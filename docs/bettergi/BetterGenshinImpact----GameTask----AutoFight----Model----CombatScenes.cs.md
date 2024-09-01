# `.\better-genshin-impact\BetterGenshinImpact\GameTask\AutoFight\Model\CombatScenes.cs`

```cs
# 引入 BetterGenshinImpact.Core.Config 命名空间，包含配置相关功能
﻿using BetterGenshinImpact.Core.Config;

# 引入 BetterGenshinImpact.Core.Recognition.OCR 命名空间，包含 OCR 相关功能
using BetterGenshinImpact.Core.Recognition.OCR;

# 引入 BetterGenshinImpact.Core.Recognition.ONNX 命名空间，包含 ONNX 相关功能
using BetterGenshinImpact.Core.Recognition.ONNX;

# 引入 BetterGenshinImpact.Core.Recognition.OpenCv 命名空间，包含 OpenCv 相关功能
using BetterGenshinImpact.Core.Recognition.OpenCv;

# 引入 BetterGenshinImpact.GameTask.AutoFight.Assets 命名空间，包含战斗任务相关资产
using BetterGenshinImpact.GameTask.AutoFight.Assets;

# 引入 BetterGenshinImpact.GameTask.AutoFight.Config 命名空间，包含战斗任务配置
using BetterGenshinImpact.GameTask.AutoFight.Config;

# 引入 BetterGenshinImpact.GameTask.Model.Area 命名空间，包含区域模型
using BetterGenshinImpact.GameTask.Model.Area;

# 引入 BetterGenshinImpact.Helpers 命名空间，包含助手功能
using BetterGenshinImpact.Helpers;

# 引入 Compunet.YoloV8 命名空间，包含 YOLOv8 相关功能
using Compunet.YoloV8;

# 引入 Microsoft.Extensions.Logging 命名空间，包含日志记录功能
using Microsoft.Extensions.Logging;

# 引入 OpenCvSharp 命名空间，包含 OpenCV 相关功能
using OpenCvSharp;

# 引入 OpenCvSharp.Extensions 命名空间，包含 OpenCV 扩展功能
using OpenCvSharp.Extensions;

# 引入 Sdcb.PaddleOCR 命名空间，包含 PaddleOCR 相关功能
using Sdcb.PaddleOCR;

# 引入 System 命名空间，包含基础系统功能
using System;

# 引入 System.Collections.Generic 命名空间，包含通用集合功能
using System.Collections.Generic;

# 引入 System.Diagnostics 命名空间，包含调试功能
using System.Diagnostics;

# 引入 System.Drawing 命名空间，包含绘图功能
using System.Drawing;

# 引入 System.Drawing.Imaging 命名空间，包含图像编码功能
using System.Drawing.Imaging;

# 引入 System.IO 命名空间，包含输入输出功能
using System.IO;

# 引入 System.Linq 命名空间，包含 LINQ 功能
using System.Linq;

# 引入 System.Threading 命名空间，包含线程功能
using System.Threading;

# 引入 BetterGenshinImpact.GameTask.Common.TaskControl 命名空间，包含任务控制功能
using static BetterGenshinImpact.GameTask.Common.TaskControl;

# 定义 BetterGenshinImpact.GameTask.AutoFight.Model 命名空间，包含战斗任务模型
namespace BetterGenshinImpact.GameTask.AutoFight.Model;

/// <summary>
/// 战斗场景
/// </summary>
public class CombatScenes : IDisposable
{
    /// <summary>
    /// 当前配队
    /// </summary>
    # 当前配队的角色数组，初始化为空的角色数组
    public Avatar[] Avatars { get; set; } = new Avatar[1];

    # 当前配队角色的字典，键为字符串，值为 Avatar 对象，初始化为空字典
    public Dictionary<string, Avatar> AvatarMap { get; set; } = [];

    # 当前配队的角色数量
    public int AvatarCount { get; set; }

    # 创建一个 YOLOv8 预测器，用于识别图像中的角色
    private readonly YoloV8Predictor _predictor =
        YoloV8Builder.CreateDefaultBuilder()
            # 使用 ONNX 模型来构建 YOLOv8 预测器
            .UseOnnxModel(Global.Absolute("Assets\\Model\\Common\\avatar_side_classify_sim.onnx"))
            # 使用 BgiSessionOption 的选项配置会话
            .WithSessionOptions(BgiSessionOption.Instance.Options)
            # 构建 YOLOv8 预测器
            .Build();

    /// <summary>
    /// 通过YOLO分类器识别队伍内角色
    /// </summary>
    /// <param name="imageRegion">完整游戏画面的捕获截图</param>
    public CombatScenes InitializeTeam(ImageRegion imageRegion)
    {
        // 优先取配置
        // 如果配置中的 TeamNames 不为空，则从配置中初始化队伍
        if (!string.IsNullOrEmpty(TaskContext.Instance().Config.AutoFightConfig.TeamNames))
        {
            InitializeTeamFromConfig(TaskContext.Instance().Config.AutoFightConfig.TeamNames);
            return this;
        }

        // 识别队伍
        // 创建两个字符串数组，用于存储队伍名称和显示名称
        var names = new string[4];
        var displayNames = new string[4];
        try
        {
            // 遍历每个头像侧边图标的矩形区域
            for (var i = 0; i < AutoFightAssets.Instance.AvatarSideIconRectList.Count; i++)
            {
                // 从图像区域裁剪出头像图像
                var ra = imageRegion.DeriveCrop(AutoFightAssets.Instance.AvatarSideIconRectList[i]);
                // 识别头像的中文名称
                var pair = ClassifyAvatarCnName(ra.SrcBitmap, i + 1);
                names[i] = pair.Item1;
                // 如果存在服装名称，则获取并格式化显示名称
                if (!string.IsNullOrEmpty(pair.Item2))
                {
                    var costumeName = pair.Item2;
                    if (AutoFightAssets.Instance.AvatarCostumeMap.TryGetValue(costumeName, out string? name))
                    {
                        costumeName = name;
                    }

                    displayNames[i] = $"{pair.Item1}({costumeName})";
                }
                else
                {
                    displayNames[i] = pair.Item1;
                }
            }
            // 记录识别到的队伍角色信息
            Logger.LogInformation("识别到的队伍角色:{Text}", string.Join(",", displayNames));
            // 根据识别的名称构建头像对象，并生成名称到头像对象的映射
            Avatars = BuildAvatars([.. names]);
            AvatarMap = Avatars.ToDictionary(x => x.Name);
        }
        catch (Exception e)
        {
            // 记录异常警告信息
            Logger.LogWarning(e.Message);
        }
        return this;
    }

    public (string, string) ClassifyAvatarCnName(Bitmap src, int index)
    {
        // 识别头像的英文名称
        var className = ClassifyAvatarName(src, index);

        var nameEn = className;
        var costumeName = "";
        // 查找服装名称在 className 中的位置
        var i = className.IndexOf("Costume", StringComparison.Ordinal);
        if (i > 0)
        {
            // 分割 className 获取英文名称和服装名称
            nameEn = className[..i];
            costumeName = className[(i + 7)..];
        }

        // 从默认配置中获取头像英文名称的映射
        var avatar = DefaultAutoFightConfig.CombatAvatarNameEnMap[nameEn];
        return (avatar.Name, costumeName);
    }

    public string ClassifyAvatarName(Bitmap src, int index)
    {
        // 创建 SpeedTimer 实例用于测量时间
        SpeedTimer speedTimer = new();
        // 使用内存流存储图像数据
        using var memoryStream = new MemoryStream();
        // 将源图像以 BMP 格式保存到内存流
        src.Save(memoryStream, ImageFormat.Bmp);
        // 将内存流的当前位置设置为开始位置
        memoryStream.Seek(0, SeekOrigin.Begin);
        // 记录角色侧面头像图像转换的时间
        speedTimer.Record("角色侧面头像图像转换");
        // 使用预测器对内存流中的图像进行分类
        var result = _predictor.Classify(memoryStream);
        // 记录角色侧面头像分类识别的时间
        speedTimer.Record("角色侧面头像分类识别");
        // 打印角色侧面头像识别结果到调试输出
        Debug.WriteLine($"角色侧面头像识别结果：{result}");
        // 打印时间测量结果
        speedTimer.DebugPrint();

        // 判断识别结果中的顶级分类名称是否以 "Qin" 开头或包含 "Costume"
        if (result.TopClass.Name.Name.StartsWith("Qin") || result.TopClass.Name.Name.Contains("Costume"))
        {
            // 降低琴和衣装角色的识别率要求
            if (result.TopClass.Confidence < 0.6)
            {
                // 将识别错误的图像保存到日志文件
                Cv2.ImWrite(@"log\avatar_side_classify_error.png", src.ToMat());
                // 抛出异常提示无法识别角色
                throw new Exception($"无法识别第{index}位角色，置信度{result.TopClass.Confidence}，结果：{result.TopClass.Name.Name}");
            }
        }
        else
        {
            // 对于其他角色，识别率要求较高
            if (result.TopClass.Confidence < 0.8)
            {
                // 将识别错误的图像保存到日志文件
                Cv2.ImWrite(@"log\avatar_side_classify_error.png", src.ToMat());
                // 抛出异常提示无法识别角色
                throw new Exception($"无法识别第{index}位角色，置信度{result.TopClass.Confidence}，结果：{result.TopClass.Name.Name}");
            }
        }

        // 返回顶级分类的名称
        return result.TopClass.Name.Name;
    }

    private void InitializeTeamFromConfig(string teamNames)
    {
        // 将队伍名称字符串按照逗号或中文逗号分隔并去除多余空格
        var names = teamNames.Split(["，", ","], StringSplitOptions.TrimEntries);
        // 检查队伍角色数量是否为4个
        if (names.Length != 4)
        {
            // 如果数量不正确，抛出异常
            throw new Exception($"强制指定队伍角色数量不正确，必须是4个，当前{names.Length}个");
        }

        // 将别名转换为标准名称
        for (var i = 0; i < names.Length; i++)
        {
            names[i] = DefaultAutoFightConfig.AvatarAliasToStandardName(names[i]);
        }

        // 记录队伍角色的标准名称
        Logger.LogInformation("强制指定队伍角色:{Text}", string.Join(",", names));
        // 设置自动战斗配置的队伍名称
        TaskContext.Instance().Config.AutoFightConfig.TeamNames = string.Join(",", names);
        // 根据名称创建角色数组
        Avatars = BuildAvatars([.. names]);
        // 将角色数组转换为角色名称到角色对象的字典
        AvatarMap = Avatars.ToDictionary(x => x.Name);
    }

    public bool CheckTeamInitialized()
    {
        // 如果 Avatars 数组中的角色少于4个，则返回 false
        if (Avatars.Length < 4)
        {
            return false;
        }

        // 否则返回 true
        return true;
    }

    private Avatar[] BuildAvatars(List<string> names, List<Rect>? nameRects = null)
    {
        // 设置角色数量
        AvatarCount = names.Count;
        // 创建与角色数量相同的 Avatar 数组
        var avatars = new Avatar[AvatarCount];
        for (var i = 0; i < AvatarCount; i++)
        {
            // 获取角色名称矩形区域，如果没有则使用空矩形
            var nameRect = nameRects?[i] ?? Rect.Empty;
            // 创建 Avatar 对象并初始化其属性
            avatars[i] = new Avatar(this, names[i], i + 1, nameRect)
            {
                // 设置角色索引矩形区域
                IndexRect = AutoFightContext.Instance.FightAssets.AvatarIndexRectList[i]
            };
        }

        // 返回创建的 Avatar 数组
        return avatars;
    }

    public void BeforeTask(CancellationTokenSource cts)
    {
        for (var i = 0; i < AvatarCount; i++)
        {
            // 为每个 Avatar 设置取消令牌源
            Avatars[i].Cts = cts;
        }
    }

    public Avatar? SelectAvatar(string name)
    {
        // 尝试从 AvatarMap 中获取指定名称的 Avatar 对象
        return AvatarMap.TryGetValue(name, out var avatar) ? avatar : null;
    }

    #region OCR识别队伍（已弃用）

    /// <summary>
    /// 通过OCR识别队伍内角色
    /// </summary>
    /// <param name="content">完整游戏画面的捕获截图</param>  # 参数说明：捕获到的游戏画面的完整截图
    [Obsolete]  # 标记此方法为过时，可能会在未来的版本中删除
    public CombatScenes InitializeTeamOldOcr(CaptureContent content)  # 公共方法，初始化团队的旧版 OCR 处理，返回当前实例
    {
        // 优先取配置
        if (!string.IsNullOrEmpty(TaskContext.Instance().Config.AutoFightConfig.TeamNames))  # 如果配置中存在团队名称
        {
            InitializeTeamFromConfig(TaskContext.Instance().Config.AutoFightConfig.TeamNames);  # 从配置中初始化团队
            return this;  # 返回当前实例
        }

        // 剪裁出队伍区域
        var teamRa = content.CaptureRectArea.DeriveCrop(AutoFightContext.Instance.FightAssets.TeamRectNoIndex);  # 从捕获的内容中剪裁出队伍区域
        // 过滤出白色
        var hsvFilterMat = OpenCvCommonHelper.InRangeHsv(teamRa.SrcMat, new Scalar(0, 0, 210), new Scalar(255, 30, 255));  # 使用 HSV 颜色空间过滤出白色区域

        // 识别队伍内角色
        var result = OcrFactory.Paddle.OcrResult(hsvFilterMat);  # 对过滤后的图像进行 OCR 识别
        ParseTeamOcrResult(result, teamRa);  # 解析 OCR 识别结果
        return this;  # 返回当前实例
    }

    [Obsolete]  # 标记此方法为过时，可能会在未来的版本中删除
    private void ParseTeamOcrResult(PaddleOcrResult result, ImageRegion rectArea)  # 私有方法，解析 OCR 结果，处理指定区域的图像
    # 定义一个字符串列表用于存储识别到的角色名字
    List<string> names = [];
    # 定义一个矩形列表用于存储角色位置的矩形区域
    List<Rect> nameRects = [];
    # 遍历识别结果中的每个区域
    foreach (var item in result.Regions)
    {
        # 提取区域文本中的中文字符
        var name = StringUtils.ExtractChinese(item.Text);
        # 对提取到的名字进行 OCR 错误修正
        name = ErrorOcrCorrection(name);
        # 判断提取的名字是否为原神角色名
        if (IsGenshinAvatarName(name))
        {
            # 将角色名字添加到名字列表
            names.Add(name);
            # 将角色区域的矩形添加到矩形列表
            nameRects.Add(item.Rect.BoundingRect());
        }
    }

    # 如果识别到的角色数量不等于 4，则记录警告日志
    if (names.Count != 4)
    {
        Logger.LogWarning("识别到的队伍角色数量不正确，当前识别结果:{Text}", string.Join(",", names));
    }

    # 如果识别到的角色数量为 3
    if (names.Count == 3)
    {
        # 尝试查找流浪者的图标
        var wanderer = rectArea.Find(AutoFightContext.Instance.FightAssets.WandererIconRa);
        # 如果流浪者图标未找到，则查找另一种流浪者图标
        if (wanderer.IsEmpty())
        {
            wanderer = rectArea.Find(AutoFightContext.Instance.FightAssets.WandererIconNoActiveRa);
        }

        # 如果仍然未找到流浪者图标
        if (wanderer.IsEmpty())
        {
            # 记录警告日志，表示二次尝试识别流浪者失败
            Logger.LogWarning("二次尝试识别失败，当前识别结果:{Text}", string.Join(",", names));
        }
        else
        {
            # 清空名字列表
            names.Clear();
            # 重新遍历识别结果中的每个区域
            foreach (var item in result.Regions)
            {
                # 提取区域文本中的中文字符
                var name = StringUtils.ExtractChinese(item.Text);
                # 对提取到的名字进行 OCR 错误修正
                name = ErrorOcrCorrection(name);
                # 判断提取的名字是否为原神角色名
                if (IsGenshinAvatarName(name))
                {
                    # 将角色名字添加到名字列表
                    names.Add(name);
                    # 将角色区域的矩形添加到矩形列表
                    nameRects.Add(item.Rect.BoundingRect());
                }

                # 获取区域的矩形
                var rect = item.Rect.BoundingRect();
                # 如果矩形在流浪者图标下方，并且不包含“流浪者”名字
                if (rect.Y > wanderer.Y && wanderer.Y + wanderer.Height > rect.Y + rect.Height && !names.Contains("流浪者"))
                {
                    # 将“流浪者”添加到名字列表
                    names.Add("流浪者");
                    # 将流浪者的区域矩形添加到矩形列表
                    nameRects.Add(item.Rect.BoundingRect());
                }
            }

            # 如果最终的角色数量不等于 4，记录警告日志
            if (names.Count != 4)
            {
                Logger.LogWarning("图像识别到流浪者，但识别队内位置信息失败");
            }
        }
    }

    # 记录识别到的队伍角色名字
    Logger.LogInformation("识别到的队伍角色:{Text}", string.Join(",", names));
    # 使用角色名字和区域矩形构建角色对象
    Avatars = BuildAvatars(names, nameRects);
    # 将角色对象映射到名字字典中
    AvatarMap = Avatars.ToDictionary(x => x.Name);



    # 标记该方法已被弃用
    [Obsolete]
    private bool IsGenshinAvatarName(string name)
    {
        # 判断名字是否在默认角色名字列表中
        if (DefaultAutoFightConfig.CombatAvatarNames.Contains(name))
        {
            return true;
        }

        return false;
    }



    # 标记该方法已被弃用
    [Obsolete]
    /// <summary>
    /// 对OCR识别结果进行纠错
    /// TODO 还剩下单字名称（魈、琴）无法识别到的问题
    /// </summary>
    /// <param name="name"></param>
    /// <returns></returns>
    public string ErrorOcrCorrection(string name)
    {
        # 如果名字中包含“纳西”，则返回“纳西妲”
        if (name.Contains("纳西"))
        {
            return "纳西妲";
        }

        # 返回原始名字
        return name;
    }



    # 结束区域，标记为 OCR 识别队伍的代码（已弃用）
    #endregion OCR识别队伍（已弃用）

    # 实现 IDisposable 接口的 Dispose 方法
    public void Dispose()
    {
        # 释放预测器资源
        _predictor.Dispose();
    }
能否提供更多代码上下文或说明该代码块的用途？这样我可以更好地理解它的作用。
```