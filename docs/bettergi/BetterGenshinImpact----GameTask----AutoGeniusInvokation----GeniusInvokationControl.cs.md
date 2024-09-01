# `.\better-genshin-impact\BetterGenshinImpact\GameTask\AutoGeniusInvokation\GeniusInvokationControl.cs`

```cs
﻿using BetterGenshinImpact.Core.Recognition.OCR;  // 引入 OCR 识别相关的命名空间
using BetterGenshinImpact.Core.Recognition.OpenCv;  // 引入 OpenCV 相关的命名空间
using BetterGenshinImpact.Core.Simulator;  // 引入模拟器相关的命名空间
using BetterGenshinImpact.GameTask.AutoGeniusInvokation.Assets;  // 引入 AutoGeniusInvokation 任务相关的资产命名空间
using BetterGenshinImpact.GameTask.AutoGeniusInvokation.Exception;  // 引入 AutoGeniusInvokation 任务相关的异常处理命名空间
using BetterGenshinImpact.GameTask.AutoGeniusInvokation.Model;  // 引入 AutoGeniusInvokation 任务相关的模型命名空间
using BetterGenshinImpact.GameTask.Common;  // 引入公共任务相关的命名空间
using BetterGenshinImpact.GameTask.Model.Area;  // 引入任务模型区域相关的命名空间
using BetterGenshinImpact.Helpers;  // 引入帮助类相关的命名空间
using BetterGenshinImpact.Helpers.Extensions;  // 引入帮助类扩展方法的命名空间
using Microsoft.Extensions.Logging;  // 引入日志记录相关的命名空间
using OpenCvSharp;  // 引入 OpenCV Sharp 的命名空间
using System;  // 引入基本系统功能的命名空间
using System.Collections.Generic;  // 引入集合相关的命名空间
using System.Diagnostics;  // 引入调试相关的命名空间
using System.Linq;  // 引入 LINQ 查询相关的命名空间
using System.Threading;  // 引入线程处理相关的命名空间
using System.Threading.Tasks;  // 引入异步任务相关的命名空间
using static BetterGenshinImpact.GameTask.Common.TaskControl;  // 引入 TaskControl 中的静态成员
using Point = OpenCvSharp.Point;  // 为 OpenCV 的 Point 类型起别名为 Point

namespace BetterGenshinImpact.GameTask.AutoGeniusInvokation;  // 定义命名空间为 BetterGenshinImpact.GameTask.AutoGeniusInvokation

/// <summary>
/// 用于操控游戏
/// </summary>
public class GeniusInvokationControl
{
    // 定义一个日志记录器，用于记录类中的日志
    private readonly ILogger<GeniusInvokationControl> _logger = App.GetLogger<GeniusInvokationControl>();

    // 定义一个静态变量来保存类的唯一实例
    private static GeniusInvokationControl? _uniqueInstance;

    // 定义一个标识以确保线程同步
    private static readonly object _locker = new();

    private AutoGeniusInvokationConfig _config;  // 定义一个配置对象用于存储 AutoGeniusInvokation 的配置

    // 定义私有构造函数，防止外部创建该类的实例
    private GeniusInvokationControl()
    {
        _config = TaskContext.Instance().Config.AutoGeniusInvokationConfig;  // 从任务上下文中获取 AutoGeniusInvokationConfig 配置
    }

    /// <summary>
    /// 定义公有方法提供一个全局访问点
    /// </summary>
    /// <returns></returns>
    public static GeniusInvokationControl GetInstance()
    {
        if (_uniqueInstance == null)  // 如果实例为空
        {
            lock (_locker)  // 获取锁以确保线程安全
            {
                _uniqueInstance ??= new GeniusInvokationControl();  // 如果实例仍然为空，则创建一个新实例
            }
        }

        return _uniqueInstance;  // 返回唯一实例
    }

    public static bool OutputImageWhenError = true;  // 定义一个静态布尔值用于决定是否在出错时输出图像

    private CancellationTokenSource? _cts;  // 定义一个可取消的任务源

    private readonly AutoGeniusInvokationAssets _assets = AutoGeniusInvokationAssets.Instance;  // 获取 AutoGeniusInvokationAssets 的唯一实例

    // private IGameCapture? _gameCapture;  // 定义一个游戏捕获接口，当前被注释掉

    public void Init(GeniusInvokationTaskParam taskParam)
    {
        _cts = taskParam.Cts;  // 初始化任务的取消源
        // _gameCapture = taskParam.Dispatcher.GameCapture;  // 初始化游戏捕获接口，当前被注释掉
    }

    public void Sleep(int millisecondsTimeout)
    {
        CheckTask();  // 检查当前任务的状态
        Thread.Sleep(millisecondsTimeout);  // 使当前线程休眠指定的毫秒数
        var sleepDelay = TaskContext.Instance().Config.AutoGeniusInvokationConfig.SleepDelay;  // 从配置中获取额外的休眠延迟时间
        if (sleepDelay > 0)  // 如果额外的休眠时间大于 0
        {
            Thread.Sleep(sleepDelay);  // 使当前线程再额外休眠指定的毫秒数
        }
    }

    public Mat CaptureGameMat()
    {
        return CaptureToRectArea().SrcMat;  // 捕获游戏区域并返回其源矩阵
    }

    public Mat CaptureGameGreyMat()
    {
        return CaptureToRectArea().SrcGreyMat;  // 捕获游戏区域并返回其灰度矩阵
    }

    public ImageRegion CaptureGameRectArea()
    {
        return CaptureToRectArea();  // 捕获游戏区域并返回区域信息
    }

    public void CheckTask()  // 方法定义的开始
    {
        // 尝试执行指定的操作，重试次数为 100，每次重试间隔 1 秒
        NewRetry.Do(() =>
        {
            // 如果取消标志已被请求，则直接返回
            if (_cts is { IsCancellationRequested: true })
            {
                return;
            }

            // 如果当前进程不是原神游戏，则记录警告并抛出重试异常
            if (!SystemControl.IsGenshinImpactActiveByProcess())
            {
                _logger.LogWarning("当前获取焦点的窗口不是原神，暂停");
                throw new RetryException("当前获取焦点的窗口不是原神");
            }
        }, TimeSpan.FromSeconds(1), 100);

        // 如果取消标志已被请求，则抛出任务取消异常
        if (_cts is { IsCancellationRequested: true })
        {
            throw new TaskCanceledException("任务取消");
        }
    }

    public void CommonDuelPrepare()
    {
        // 1. 选择初始手牌
        Sleep(1000); // 等待 1 秒
        _logger.LogInformation("开始选择初始手牌"); // 记录信息：开始选择初始手牌
        while (!ClickConfirm())
        {
            // 循环等待选择卡牌画面，直到点击确认成功
            Sleep(1000); // 等待 1 秒
        }

        _logger.LogInformation("点击确认"); // 记录信息：点击确认

        // 2. 选择出战角色
        // 此处选择第2个角色 雷神
        _logger.LogInformation("等待3s对局准备..."); // 记录信息：等待 3 秒准备对局
        Sleep(3000); // 等待 3 秒

        // 是否是再角色出战选择界面，重试 20 次，每次间隔 0.8 秒
        NewRetry.Do(IsInCharacterPickRetryThrowable, TimeSpan.FromSeconds(0.8), 20);
        _logger.LogInformation("识别到已经在角色出战界面，等待1.5s"); // 记录信息：识别到角色出战界面，等待 1.5 秒
        Sleep(1500); // 等待 1.5 秒
    }

    public void SortActionPhaseDiceMats(HashSet<ElementalType> elementSet)
    {
        // 按照元素类型排序动作阶段骰子材料，并转换为字典
        _assets.ActionPhaseDiceMats = _assets.ActionPhaseDiceMats.OrderByDescending(kvp =>
            {
                for (var i = 0; i < elementSet.Count; i++)
                {
                    // 如果骰子类型匹配元素集合中的类型，返回其索引
                    if (kvp.Key == elementSet.ElementAt(i).ToLowerString())
                    {
                        return i;
                    }
                }

                // 如果未匹配任何元素，返回 -1
                return -1;
            })
            .ToDictionary(x => x.Key, x => x.Value); // 转换为字典
        // 打印排序后的顺序
        var msg = _assets.ActionPhaseDiceMats.Aggregate("", (current, kvp) => current + $"{kvp.Key.ToElementalType().ToChinese()}| ");
        _logger.LogDebug("当前骰子排序：{Msg}", msg); // 记录调试信息：当前骰子排序
    }

    /// <summary>
    /// 获取我方三个角色卡牌区域
    /// </summary>
    /// <returns></returns>
    public List<Rect> GetCharacterRects()
    {
        // 需要实现：返回我方三个角色的卡牌区域
    }

    /// <summary>
    /// 点击捕获区域的相对位置
    /// </summary>
    /// <param name="x"></param>
    /// <param name="y"></param>
    /// <returns></returns>
    public void ClickCaptureArea(int x, int y)
    {
        var rect = TaskContext.Instance().SystemInfo.CaptureAreaRect; // 获取捕获区域的矩形
        ClickExtension.Click(rect.X + x, rect.Y + y); // 在相对位置点击
    }

    /// <summary>
    ///  点击游戏屏幕中心点
    /// </summary>
    public void ClickGameWindowCenter()
    {
        var p = TaskContext.Instance().SystemInfo.CaptureAreaRect.GetCenterPoint(); // 获取捕获区域的中心点
        p.Click(); // 点击中心点
    }

    /*public static Dictionary<string, List<Point>> FindMultiPicFromOneImage(Mat srcMat, Dictionary<string, Mat> imgSubDictionary, double threshold = 0.8)
    {
        // 需要实现：从源图像中查找多个子图像，并返回字典
    */
    {
        // 创建一个新的字典，用于存储模板匹配的结果，字典的键是模板的名称，值是匹配到的点的列表
        var dictionary = new Dictionary<string, List<Point>>();
        // 遍历所有待匹配的模板
        foreach (var kvp in imgSubDictionary)
        {
            // 对当前模板进行多次匹配，返回匹配到的点的列表
            var list = MatchTemplateHelper.MatchTemplateMulti(srcMat, kvp.Value, threshold);
            // 将匹配结果添加到字典中，键是模板名称，值是匹配点的列表
            dictionary.Add(kvp.Key, list);
            // 把结果给遮掩掉，避免重复识别
            foreach (var point in list)
            {
                // 在源图像上绘制一个黑色矩形来遮盖匹配结果
                Cv2.Rectangle(srcMat, point, new Point(point.X + kvp.Value.Width, point.Y + kvp.Value.Height), Scalar.Black, -1);
            }
        }

        // 返回包含所有模板匹配结果的字典
        return dictionary;
    }*/

    // 定义一个静态方法，用于从源图像中逐个查找多个模板，返回匹配点的字典
    public static Dictionary<string, List<Point>> FindMultiPicFromOneImage2OneByOne(Mat srcMat, Dictionary<string, Mat> imgSubDictionary, double threshold = 0.8)
    {
        // 创建一个新的字典，用于存储模板匹配的结果，字典的键是模板的名称，值是匹配到的点的列表
        var dictionary = new Dictionary<string, List<Point>>();
        // 遍历所有待匹配的模板
        foreach (var kvp in imgSubDictionary)
        {
            // 创建一个新的列表，用于存储当前模板的匹配点
            var list = new List<Point>();

            // 不断匹配模板，直到没有找到新的匹配点
            while (true)
            {
                // 执行模板匹配，返回匹配到的点
                var point = MatchTemplateHelper.MatchTemplate(srcMat, kvp.Value, TemplateMatchModes.CCoeffNormed, null, threshold);
                // 如果找到了匹配点
                if (point != new Point())
                {
                    // 把结果给遮掩掉，避免重复识别
                    Cv2.Rectangle(srcMat, point, new Point(point.X + kvp.Value.Width, point.Y + kvp.Value.Height), Scalar.Black, -1);
                    // 将匹配点添加到列表中
                    list.Add(point);
                }
                // 如果没有找到匹配点，则退出循环
                else
                {
                    break;
                }
            }

            // 将当前模板的匹配结果添加到字典中
            dictionary.Add(kvp.Key, list);
        }

        // 返回包含所有模板匹配结果的字典
        return dictionary;
    }

    /// <summary>
    /// 重投骰子
    /// </summary>
    /// <param name="holdElementalTypes">保留的元素类型</param>
    public bool RollPhaseReRoll(params ElementalType[] holdElementalTypes)
    # 捕获当前游戏截图
    var gameSnapshot = CaptureGameMat();
    # 将截图的颜色格式从 BGRA 转换为 BGR
    Cv2.CvtColor(gameSnapshot, gameSnapshot, ColorConversionCodes.BGRA2BGR);
    # 从截图中寻找图像匹配，返回包含匹配结果的字典
    var dictionary = FindMultiPicFromOneImage2OneByOne(gameSnapshot, _assets.RollPhaseDiceMats, 0.73);

    # 计算字典中所有匹配图像的总数
    var count = dictionary.Sum(kvp => kvp.Value.Count);

    # 如果骰子数量不等于8，则记录调试信息并返回 false
    if (count != 8)
    {
        _logger.LogDebug("投骰子界面识别到了{Count}个骰子,等待重试", count);
        return false;
    }
    else
    {
        # 如果骰子数量正确，记录信息
        _logger.LogInformation("投骰子界面识别到了{Count}个骰子", count);
    }

    # 初始化骰子位置计数器
    int upper = 0, lower = 0;
    # 遍历字典，计算上半部分和下半部分骰子的数量
    foreach (var kvp in dictionary)
    {
        foreach (var point in kvp.Value)
        {
            if (point.Y < gameSnapshot.Height / 2)
            {
                upper++;
            }
            else
            {
                lower++;
            }
        }
    }

    # 如果上半部分和下半部分骰子数量不等于4，则记录信息并返回 false
    if (upper != 4 || lower != 4)
    {
        _logger.LogInformation("骰子识别位置错误,重试");
        return false;
    }

    # 遍历字典，处理每个图像的点击操作
    foreach (var kvp in dictionary)
    {
        # 跳过指定的保留元素类型
        if (holdElementalTypes.Contains(kvp.Key.ToElementalType()))
        {
            continue;
        }

        # 对每个图像位置执行点击操作，并稍作休眠
        foreach (var point in kvp.Value)
        {
            ClickCaptureArea(point.X + _assets.RollPhaseDiceMats[kvp.Key].Width / 2, point.Y + _assets.RollPhaseDiceMats[kvp.Key].Height / 2);
            Sleep(100);
        }
    }

    # 返回操作成功的标志
    return true;
    # 查找并点击确认按钮
    public bool ClickConfirm()
    {
        # 捕获游戏区域并寻找确认按钮
        var foundRectArea = CaptureGameRectArea().Find(_assets.ConfirmButtonRo);
        # 如果找到确认按钮，则点击并释放资源，返回 true
        if (!foundRectArea.IsEmpty())
        {
            foundRectArea.Click();
            foundRectArea.Dispose();
            return true;
        }

        # 如果未找到确认按钮，返回 false
        return false;
    }

    # 重投骰子操作
    public void ReRollDice(params ElementalType[] holdElementalTypes)
    {
        # 记录重投前的等待时间
        _logger.LogInformation("等待5s投骰动画...");

        # 生成保留骰子的消息字符串
        var msg = holdElementalTypes.Aggregate(" ", (current, elementalType) => current + (elementalType.ToChinese() + " "));

        # 记录保留骰子的信息
        _logger.LogInformation("保留{Msg}骰子", msg);
        # 等待5秒钟
        Sleep(5000);
        # 初始化重试计数器
        var retryCount = 0;
        # 在保留指定骰子的情况下执行重投操作，直到成功
        while (!RollPhaseReRoll(holdElementalTypes))
        {
            retryCount++;

            # 如果对战结束，则抛出异常
            if (IsDuelEnd())
            {
                throw new NormalEndException("对战已结束,停止自动打牌！");
            }

            # 如果重试次数过多，抛出异常
            Sleep(500);
            if (retryCount > 35)
            {
                throw new System.Exception("识别骰子数量不正确,重试超时,停止自动打牌！");
            }
        }

        # 点击确认按钮
        ClickConfirm();
        _logger.LogInformation("选择需要重投的骰子后点击确认完毕");

        # 等待1秒钟
        Sleep(1000);
        # 移动鼠标到游戏窗口中心
        ClickGameWindowCenter();

        # 记录等待对方重投的时间
        _logger.LogInformation("等待5s对方重投");
        Sleep(5000);
    }
    // 创建一个新的 Point 对象，位置偏移量为传入的 p
    public Point MakeOffset(Point p)
    {
        // 获取当前任务上下文中的系统信息的捕捉区域矩形
        var rect = TaskContext.Instance().SystemInfo.CaptureAreaRect;
        // 返回一个新的 Point 对象，位置为矩形的左上角加上传入的偏移量
        return new Point(rect.X + p.X, rect.Y + p.Y);
    }

    /// <summary>
    /// 计算当前有那些骰子
    /// </summary>
    /// <returns></returns>
    public Dictionary<string, int> ActionPhaseDice()
    {
        // 捕捉游戏画面
        var srcMat = CaptureGameMat();
        // 将捕捉的图像从 BGRA 转换为 BGR 格式
        Cv2.CvtColor(srcMat, srcMat, ColorConversionCodes.BGRA2BGR);
        // 切割图片右侧部分以加快识别速度，并对切割后的图像进行多图片识别
        var dictionary = FindMultiPicFromOneImage2OneByOne(CutRight(srcMat, srcMat.Width / 5), _assets.ActionPhaseDiceMats, 0.7);

        var msg = "";
        var result = new Dictionary<string, int>();
        // 遍历识别到的图片字典
        foreach (var kvp in dictionary)
        {
            // 将识别到的骰子类型及其数量添加到结果字典
            result.Add(kvp.Key, kvp.Value.Count);
            // 拼接消息字符串，包含骰子类型和数量
            msg += $"{kvp.Key.ToElementalType().ToChinese()} {kvp.Value.Count}| ";
        }

        // 记录当前骰子状态信息
        _logger.LogInformation("当前骰子状态：{Res}", msg);
        // 返回结果字典
        return result;
    }

    /// <summary>
    /// 烧牌
    /// </summary>
    public void ActionPhaseElementalTuning(int currentCardCount)
    {
        // 获取当前任务上下文中的系统信息的捕捉区域矩形
        var rect = TaskContext.Instance().SystemInfo.CaptureAreaRect;
        var m = Simulation.SendInput.Mouse;
        // 点击捕捉区域矩形的中心下方的某个位置
        ClickExtension.Click(rect.X + rect.Width / 2d, rect.Y + rect.Height - 50);
        Sleep(1500); // 等待 1500 毫秒

        if (currentCardCount == 1)
        {
            // 如果当前牌数为 1，点击右侧的牌位置
            ClickExtension.Move(rect.X + rect.Width / 2d + 120, rect.Y + rect.Height - 50);
        }

        m.LeftButtonDown(); // 按下鼠标左键
        Sleep(100); // 等待 100 毫秒
        // 移动鼠标到捕捉区域矩形右下角的某个位置
        m = ClickExtension.Move(rect.X + rect.Width - 50, rect.Y + rect.Height / 2d);
        Sleep(100); // 等待 100 毫秒
        m.LeftButtonUp(); // 松开鼠标左键
    }

    /// <summary>
    /// 烧牌确认（元素调和按钮）
    /// </summary>
    public bool ActionPhaseElementalTuningConfirm()
    {
        // 捕捉游戏区域
        var ra = CaptureGameRectArea();
        // 在捕捉区域中查找元素调和确认按钮的区域
        var foundRectArea = ra.Find(_assets.ElementalTuningConfirmButtonRo);
        if (!foundRectArea.IsEmpty())
        {
            // 如果找到了确认按钮区域，点击它并返回 true
            foundRectArea.Click();
            return true;
        }

        // 如果没有找到确认按钮区域，返回 false
        return false;
    }

    /// <summary>
    /// 点击切人按钮
    /// </summary>
    /// <returns></returns>
    public void ActionPhasePressSwitchButton()
    {
        // 获取当前任务上下文中的系统信息
        var info = TaskContext.Instance().SystemInfo;
        // 计算切人按钮的坐标
        var x = info.CaptureAreaRect.X + info.CaptureAreaRect.Width - 100 * info.AssetScale;
        var y = info.CaptureAreaRect.Y + info.CaptureAreaRect.Height - 120 * info.AssetScale;

        // 移动鼠标到切人按钮位置并点击
        ClickExtension.Move(x, y).LeftButtonClick();
        Sleep(800); // 等待 800 毫秒，以确保动画完全播放

        // 再次点击切人按钮
        ClickExtension.Move(x, y).LeftButtonClick();
    }

    /// <summary>
    /// 使用技能
    /// </summary>
    /// <param name="skillIndex">技能编号,从右往左数,从1开始</param>
    /// <returns>元素骰子是否充足</returns>
    public bool ActionPhaseUseSkill(int skillIndex)
    {
        ClickGameWindowCenter(); // 复位窗口中心位置
        Sleep(500); // 暂停500毫秒，确保操作完成
        // 计算技能点击坐标，坐标取决于技能索引和缩放比例
        var info = TaskContext.Instance().SystemInfo;
        var x = info.CaptureAreaRect.X + info.CaptureAreaRect.Width - 100 * info.AssetScale * skillIndex;
        var y = info.CaptureAreaRect.Y + info.CaptureAreaRect.Height - 120 * info.AssetScale;
        ClickExtension.Click(x, y); // 点击计算得到的坐标位置
        Sleep(1200); // 等待1200毫秒，确保动画彻底弹出

        // 查找元素骰子不足的警告
        var foundRectArea = CaptureGameRectArea().Find(_assets.ElementalDiceLackWarningRo);
        if (foundRectArea.IsEmpty()) // 如果警告未找到
        {
            // 再次点击以确保技能被使用
            _logger.LogInformation("使用技能{SkillIndex}", skillIndex);
            ClickExtension.Click(x, y); // 再次点击计算得到的坐标位置
            Sleep(500); // 等待500毫秒
            ClickGameWindowCenter(); // 复位窗口中心位置
            return true; // 返回技能使用成功
        }

        return false; // 返回技能未成功使用
    }

    /// <summary>
    /// 使用技能（元素骰子不够的情况下，自动烧牌）
    /// </summary>
    /// <param name="skillIndex">技能编号,从右往左数,从1开始</param>
    /// <param name="diceCost">技能消耗骰子数</param>
    /// <param name="elementalType">消耗骰子元素类型</param>
    /// <param name="duel">对局对象</param>
    /// <returns>手牌或者元素骰子是否充足</returns>
    public bool ActionPhaseAutoUseSkill(int skillIndex, int diceCost, ElementalType elementalType, Duel duel)
    }

    /// <summary>
    /// 回合结束
    /// </summary>
    public void RoundEnd()
    {
        // 查找回合结束按钮并点击
        CaptureGameRectArea().Find(_assets.RoundEndButtonRo, foundRectArea =>
        {
            foundRectArea.Click(); // 点击找到的区域
            Sleep(1000); // 等待1000毫秒，有弹出动画
            foundRectArea.Click(); // 再次点击以确认操作
            Sleep(300); // 等待300毫秒
        });

        ClickGameWindowCenter(); // 复位窗口中心位置
    }

    /// <summary>
    /// 是否是再角色出战选择界面
    /// 可重试方法
    /// </summary>
    public void IsInCharacterPickRetryThrowable()
    {
        if (!IsInCharacterPick()) // 如果不在角色选择界面
        {
            throw new RetryException("当前不在角色出战选择界面"); // 抛出重试异常
        }
    }

    /// <summary>
    /// 是否是再角色出战选择界面
    /// </summary>
    /// <returns></returns>
    public bool IsInCharacterPick()
    {
        return !CaptureGameRectArea().Find(_assets.InCharacterPickRo).IsEmpty(); // 返回是否在角色选择界面
    }

    /// <summary>
    /// 是否是我的回合
    /// </summary>
    /// <returns></returns>
    public bool IsInMyAction()
    {
        return !CaptureGameRectArea().Find(_assets.RoundEndButtonRo).IsEmpty(); // 返回是否是我的回合
    }

    /// <summary>
    /// 是否是对方的回合
    /// </summary>
    /// <returns></returns>
    public bool IsInOpponentAction()
    {
        return !CaptureGameRectArea().Find(_assets.InOpponentActionRo).IsEmpty(); // 返回是否是对方的回合
    }

    /// <summary>
    /// 是否是回合结算阶段
    /// </summary>
    /// <returns></returns>
    public bool IsEndPhase()
    {
        return !CaptureGameRectArea().Find(_assets.EndPhaseRo).IsEmpty(); // 返回是否是回合结算阶段
    }

    /// <summary>
    /// 出战角色是否被打倒
    /// </summary>
    /// <returns></returns>
    public bool IsActiveCharacterTakenOut()
    {
        return !CaptureGameRectArea().Find(_assets.CharacterTakenOutRo).IsEmpty(); // 返回出战角色是否被打倒
    }

    /// <summary>
    /// 哪些出战角色被打倒了
    /// </summary>
    /// <returns>true 是已经被打倒</returns>
    public bool[] WhatCharacterDefeated(List<Rect> rects)
    {
        // 如果传入的 rects 为 null 或者其元素个数不是 3，则抛出异常
        if (rects == null || rects.Count != 3)
        {
            throw new System.Exception("未能获取到我方角色卡位置");
        }

        // 使用模板匹配检测已经被打倒的角色
        var pList = MatchTemplateHelper.MatchTemplateMulti(CaptureGameGreyMat(), _assets.CharacterDefeatedMat, 0.8);

        // 初始化布尔数组用于存储每个角色是否被打倒的结果
        var res = new bool[3];
        foreach (var p in pList)
        {
            for (var i = 0; i < rects.Count; i++)
            {
                // 检查当前检测到的区域是否与角色卡片区域重叠
                if (IsOverlap(rects[i], new Rect(p.X, p.Y, _assets.CharacterDefeatedMat.Width, _assets.CharacterDefeatedMat.Height)))
                {
                    // 如果重叠，标记该角色已被打倒
                    res[i] = true;
                }
            }
        }

        // 返回每个角色是否被打倒的布尔数组
        return res;
    }

    /// <summary>
    /// 判断矩形是否重叠
    /// </summary>
    /// <param name="rc1"></param>
    /// <param name="rc2"></param>
    /// <returns></returns>
    public bool IsOverlap(Rect rc1, Rect rc2)
    {
        // 检查两个矩形是否有重叠区域
        if (rc1.X + rc1.Width > rc2.X &&
            rc2.X + rc2.Width > rc1.X &&
            rc1.Y + rc1.Height > rc2.Y &&
            rc2.Y + rc2.Height > rc1.Y
           )
        {
            return true; // 矩形重叠
        }
        else
        {
            return false; // 矩形不重叠
        }
    }

    /// <summary>
    /// 是否对局完全结束
    /// </summary>
    /// <returns></returns>
    public bool IsDuelEnd()
    {
        // 检查是否能找到结束对局的按钮区域
        return !CaptureGameRectArea().Find(_assets.ExitDuelButtonRo).IsEmpty();
    }

    public Mat CutRight(Mat srcMat, int saveRightWidth)
    {
        // 从源图像中裁剪出右侧指定宽度的区域
        srcMat = new Mat(srcMat, new Rect(srcMat.Width - saveRightWidth, 0, saveRightWidth, srcMat.Height));
        return srcMat;
    }

    /// <summary>
    /// 等待我的回合
    /// 我方角色可能在此期间阵亡
    /// </summary>
    public void WaitForMyTurn(Duel duel, int waitTime = 0)
    {
        // 如果 waitTime 大于 0，等待指定时间再继续执行
        if (waitTime > 0)
        {
            _logger.LogInformation("等待对方行动{Time}s", waitTime / 1000);
            Sleep(waitTime);
        }

        // 循环检查对方的行动状态
        var retryCount = 0;
        var inMyActionCount = 0;
        while (true)
        {
            if (IsInMyAction())
            {
                if (IsActiveCharacterTakenOut())
                {
                    // 如果我方角色被打倒，执行相应操作
                    DoWhenCharacterDefeated(duel);
                }
                else
                {
                    // 如果我方角色没有被打倒，增加延迟并重试
                    inMyActionCount++;
                    if (inMyActionCount == 3)
                    {
                        break; // 如果尝试次数达到 3 次，则退出循环
                    }
                }
            }
            else if (IsDuelEnd())
            {
                // 如果对局结束，抛出异常
                throw new NormalEndException("对战已结束,停止自动打牌！");
            }

            retryCount++;
            if (retryCount >= 60)
            {
                // 如果重试次数超过 60 次，抛出异常
                throw new System.Exception("等待对方行动超时,停止自动打牌！");
            }

            // 记录当前等待状态，并继续等待
            _logger.LogInformation("对方仍在行动中,继续等待(次数{Count})...", retryCount);
            Sleep(1000);
        }
    }

    /// <summary>
    /// 等待对方回合 和 回合结束阶段
    /// 我方角色可能在此期间阵亡
    /// </summary>
    // 等待对方行动并处理相关逻辑
    public void WaitOpponentAction(Duel duel)
    {
        // 创建随机数生成器
        var rd = new Random();
        // 随机等待 3000 毫秒至 4000 毫秒
        Sleep(3000 + rd.Next(1, 1000));
        // 初始化重试次数计数器
        var retryCount = 0;
        // 无限循环，直到条件满足跳出
        while (true)
        {
            // 如果对方仍在行动中
            if (IsInOpponentAction())
            {
                // 记录对方仍在行动中，继续等待
                _logger.LogInformation("对方仍在行动中,继续等待(次数{Count})...", retryCount);
            }
            // 如果当前处于回合结束阶段
            else if (IsEndPhase())
            {
                // 记录当前处于回合结束阶段，继续等待
                _logger.LogInformation("正在回合结束阶段,继续等待(次数{Count})...", retryCount);
            }
            // 如果当前处于我的行动阶段
            else if (IsInMyAction())
            {
                // 如果角色已经被打败
                if (IsActiveCharacterTakenOut())
                {
                    // 处理角色被打败的情况
                    DoWhenCharacterDefeated(duel);
                }
            }
            // 如果对战已经结束
            else if (IsDuelEnd())
            {
                // 抛出异常，表示对战已结束
                throw new NormalEndException("对战已结束,停止自动打牌！");
            }
            // 如果没有符合以上条件
            else
            {
                // 如果重试次数超过 2 次，则跳出循环
                if (retryCount > 2)
                {
                    break;
                }
                else
                {
                    // 记录等待对方回合和回合结束阶段时程序未识别到有效内容
                    _logger.LogError("等待对方回合 和 回合结束阶段 时程序未识别到有效内容(次数{Count})...", retryCount);
                }
            }

            // 增加重试次数
            retryCount++;
            // 如果重试次数达到 60 次，抛出超时异常
            if (retryCount >= 60)
            {
                throw new System.Exception("等待对方行动超时,停止自动打牌！");
            }

            // 随机等待 1000 毫秒至 1500 毫秒
            Sleep(1000 + rd.Next(1, 500));
        }
    }

    /// <summary>
    /// 角色被打败后要切换角色
    /// </summary>
    /// <param name="duel"></param>
    /// <exception cref="NormalEndException"></exception>
    public void DoWhenCharacterDefeated(Duel duel)
    {
        // 记录当前角色被打败，需要选择新的出战角色
        _logger.LogInformation("当前出战角色被打败，需要选择新的出战角色");
        // 获取被打败角色的数组
        var defeatedArray = WhatCharacterDefeated(duel.CharacterCardRects);

        // 从后往前遍历被打败角色数组
        for (var i = defeatedArray.Length - 1; i >= 0; i--)
        {
            // 更新角色的被打败状态
            duel.Characters[i + 1].IsDefeated = defeatedArray[i];
        }

        // 获取角色切换的顺序列表
        var orderList = duel.GetCharacterSwitchOrder();
        // 如果切换顺序列表为空，抛出异常
        if (orderList.Count == 0)
        {
            throw new NormalEndException("后续行动策略中,已经没有可切换且存活的角色了,结束自动打牌(建议添加更多行动)");
        }

        // 遍历角色切换顺序列表
        foreach (var j in orderList)
        {
            // 如果角色未被打败
            if (!duel.Characters[j].IsDefeated)
            {
                // 执行角色切换操作
                duel.Characters[j].SwitchWhenTakenOut();
                // 切换角色后跳出循环
                break;
            }
        }

        // 点击游戏窗口中心
        ClickGameWindowCenter();
        // 等待 2000 毫秒，处理切换角色的动画
        Sleep(2000); // 切人动画
    }

    // 添加角色状态的方法（缺少实现）
    public void AppendCharacterStatus(Character character, Mat greyMat, int hp = -2)
    {
        // 截取出战角色区域扩展，创建一个新矩阵用于角色状态识别
        using var characterMat = new Mat(greyMat, new Rect(character.Area.X,
            character.Area.Y,
            character.Area.Width + 40,
            character.Area.Height + 10));
        // 使用模板匹配识别角色是否被冻结
        var pCharacterStatusFreeze = MatchTemplateHelper.MatchTemplate(characterMat, _assets.CharacterStatusFreezeMat, TemplateMatchModes.CCoeffNormed);
        // 如果匹配结果不为空，说明角色被冻结
        if (pCharacterStatusFreeze != new Point())
        {
            character.StatusList.Add(CharacterStatusEnum.Frozen);
        }

        // 使用模板匹配识别角色是否眩晕
        var pCharacterStatusDizziness = MatchTemplateHelper.MatchTemplate(characterMat, _assets.CharacterStatusDizzinessMat, TemplateMatchModes.CCoeffNormed);
        // 如果匹配结果不为空，说明角色眩晕
        if (pCharacterStatusDizziness != new Point())
        {
            character.StatusList.Add(CharacterStatusEnum.Frozen);
        }

        // 使用模板匹配识别角色的能量点
        var energyPointList = MatchTemplateHelper.MatchTemplateMulti(characterMat.Clone(), _assets.CharacterEnergyOnMat, 0.8);
        // 根据识别到的能量点数量更新角色的能量值
        character.EnergyByRecognition = energyPointList.Count;

        // 更新角色的血量值
        character.Hp = hp;

        // 记录当前角色状态信息到日志
        _logger.LogInformation("当前出战{Character}", character);
    }

    public Character WhichCharacterActiveWithRetry(Duel duel)
    {
        // 检查角色是否被击败，这里再检查一次是因为最后一个角色存活的情况下，会自动出战
        var defeatedArray = WhatCharacterDefeated(duel.CharacterCardRects);
        // 更新每个角色的击败状态
        for (var i = defeatedArray.Length - 1; i >= 0; i--)
        {
            duel.Characters[i + 1].IsDefeated = defeatedArray[i];
        }

        // 根据血量 OCR 识别哪个角色处于激活状态
        return WhichCharacterActiveByHpOcr(duel);
    }

    public Character WhichCharacterActiveByHpWord(Duel duel)
    {
        # 检查角色卡位置是否为空或数量不为3
        if (duel.CharacterCardRects == null || duel.CharacterCardRects.Count != 3)
        {
            # 如果条件不满足，抛出异常提示未能获取角色卡位置
            throw new System.Exception("未能获取到我方角色卡位置");
        }

        # 捕获游戏画面的矩阵
        var srcMat = CaptureGameMat();

        # 计算图像高度的一半
        int halfHeight = srcMat.Height / 2;
        # 从源图像中提取下半部分的矩阵
        Mat bottomMat = new(srcMat, new Rect(0, halfHeight, srcMat.Width, srcMat.Height - halfHeight));

        # 定义紫色的低阈值和高阈值
        var lowPurple = new Scalar(239, 239, 239);
        var highPurple = new Scalar(255, 255, 255);
        # 将下半部分矩阵进行阈值处理，转为灰度图像
        Mat gray = OpenCvCommonHelper.Threshold(bottomMat, lowPurple, highPurple);

        # 定义结构元素用于膨胀操作
        var kernel = Cv2.GetStructuringElement(MorphShapes.Rect, new OpenCvSharp.Size(15, 10), new OpenCvSharp.Point(-1, -1));
        # 对灰度图像进行膨胀操作
        Cv2.Dilate(gray, gray, kernel); #膨胀

        # 查找轮廓
        Cv2.FindContours(gray, out var contours, out _, RetrievalModes.External,
            ContourApproximationModes.ApproxSimple, null);

        # 如果找到轮廓
        if (contours.Length > 0)
        {
            # 计算轮廓的边界矩形，按Y轴高度排序
            var rects = contours
                .Select(Cv2.BoundingRect)
                # 按照Y轴高度排序
                .OrderBy(r => r.Y)
                .ToList();

            # 遍历每个边界矩形
            foreach (var rect in rects)
            {
                for (var i = 0; i < duel.CharacterCardRects.Count; i++)
                {
                    # 扩展矩形的高度和宽度，以确保能够相交
                    var rect1 = new Rect(rect.X, halfHeight + rect.Y, rect.Width + 20,
                        rect.Height + 20);
                    # 检查扩展矩形是否与角色卡位置重叠且位置在角色卡上方
                    if (IsOverlap(rect1, duel.CharacterCardRects[i]) &&
                        halfHeight + rect.Y < duel.CharacterCardRects[i].Y)
                    {
                        # 将当前角色设置为出战角色
                        duel.CurrentCharacter = duel.Characters[i + 1];
                        var grayMat = new Mat();
                        # 将源图像转换为灰度图像
                        Cv2.CvtColor(srcMat, grayMat, ColorConversionCodes.BGR2GRAY);
                        # 附加当前角色的状态信息
                        AppendCharacterStatus(duel.CurrentCharacter, grayMat);

                        # 在源图像上绘制矩形框
                        Cv2.Rectangle(srcMat, rect1, Scalar.Yellow);
                        Cv2.Rectangle(srcMat, duel.CharacterCardRects[i], Scalar.Blue, 2);
                        # 输出图像到指定路径
                        OutputImage(duel, rects, bottomMat, halfHeight, "log\\active_character2_success.jpg");
                        # 返回当前角色
                        return duel.CurrentCharacter;
                    }
                }
            }

            # 输出图像到指定路径，记录没有重叠的错误
            OutputImage(duel, rects, bottomMat, halfHeight, "log\\active_character2_no_overlap_error.jpg");
        }
        else
        {
            # 如果未找到轮廓且需要输出错误图像
            if (OutputImageWhenError)
            {
                # 保存灰度图像以记录没有找到矩形的错误
                Cv2.ImWrite("log\\active_character2_no_rects_error.jpg", gray);
            }
        }

        # 如果未能识别到出战角色，抛出重试异常
        throw new RetryException("未识别到个出战角色");
    }

    # 定义方法来根据生命值OCR识别出战角色
    public Character WhichCharacterActiveByHpOcr(Duel duel)
    {
        # 检查角色卡的矩形列表是否为 null 或者数量不是 3
        if (duel.CharacterCardRects == null || duel.CharacterCardRects.Count != 3)
        {
            # 抛出异常，说明未能获取到正确的角色卡位置
            throw new System.Exception("未能获取到我方角色卡位置");
        }

        # 捕获游戏当前灰度图像
        var srcMat = CaptureGameGreyMat();

        # 初始化数组，1 代表未出战，2 代表出战
        var hpArray = new int[3];
        for (var i = 0; i < duel.CharacterCardRects.Count; i++)
        {
            # 如果角色已经被击败，则标记为未出战
            if (duel.Characters[i + 1].IsDefeated)
            {
                hpArray[i] = 1;
                continue;
            }

            # 获取当前角色的矩形区域
            var cardRect = duel.CharacterCardRects[i];
            # 从源图像中提取角色的 HP 区域
            var hpMat = new Mat(srcMat, new Rect(cardRect.X + _config.CharacterCardExtendHpRect.X,
                cardRect.Y + _config.CharacterCardExtendHpRect.Y,
                _config.CharacterCardExtendHpRect.Width, _config.CharacterCardExtendHpRect.Height));
            # 通过 OCR 识别 HP 区域的文本
            var text = OcrFactory.Paddle.Ocr(hpMat);
            # 输出识别结果到调试日志
            Debug.WriteLine($"角色{i}未出战HP位置识别结果{text}");
            if (!string.IsNullOrWhiteSpace(text))
            {
                # 如果识别到文本，说明角色未出战
                hpArray[i] = 1;
            }
            else
            {
                # 如果没有识别到文本，调整区域位置并重新进行 OCR
                hpMat = new Mat(srcMat, new Rect(cardRect.X + _config.CharacterCardExtendHpRect.X,
                    cardRect.Y + _config.CharacterCardExtendHpRect.Y - _config.ActiveCharacterCardSpace,
                    _config.CharacterCardExtendHpRect.Width, _config.CharacterCardExtendHpRect.Height));
                text = OcrFactory.Paddle.Ocr(hpMat);
                # 输出识别结果到调试日志
                Debug.WriteLine($"角色{i}出战HP位置识别结果{text}");
                if (!string.IsNullOrWhiteSpace(text))
                {
                    # 如果识别到文本，更新 HP 数值，并标记角色为出战
                    var hp = -2;
                    if (RegexHelper.FullNumberRegex().IsMatch(text))
                    {
                        hp = int.Parse(text);
                    }

                    hpArray[i] = 2;
                    duel.CurrentCharacter = duel.Characters[i + 1];
                    AppendCharacterStatus(duel.CurrentCharacter, srcMat, hp);
                    # 返回当前角色
                    return duel.CurrentCharacter;
                }
            }
        }

        # 如果找到两个未出战的角色，使用排除法确定出战角色
        if (hpArray.Count(x => x == 1) == 2)
        {
            var index = hpArray.ToList().FindIndex(x => x != 1);
            Debug.WriteLine($"通过OCR HP的方式没有识别到出战角色，但是通过排除法确认角色{index + 1}处于出战状态！");
            duel.CurrentCharacter = duel.Characters[index + 1];
            AppendCharacterStatus(duel.CurrentCharacter, srcMat);
            # 返回当前角色
            return duel.CurrentCharacter;
        }

        # 如果上述方法都未能识别出战角色，进行重试
        _logger.LogWarning("通过OCR HP的方式未识别到出战角色 {Array}", hpArray);
        return NewRetry.Do(() => WhichCharacterActiveByHpWord(duel), TimeSpan.FromSeconds(0.3), 2);
    }

    # 声明一个静态方法，输出图像
    private static void OutputImage(Duel duel, List<Rect> rects, Mat bottomMat, int halfHeight, string fileName)
    {
        # 如果发生错误时需要输出图像
        if (OutputImageWhenError)
        {
            # 遍历所有矩形区域 rects
            foreach (var rect2 in rects)
            {
                # 在图像上绘制红色矩形，表示错误区域
                Cv2.Rectangle(bottomMat, new OpenCvSharp.Point(rect2.X, rect2.Y),
                    new OpenCvSharp.Point(rect2.X + rect2.Width, rect2.Y + rect2.Height), Scalar.Red, 1);
            }

            # 遍历所有角色卡片的矩形区域
            foreach (var rc in duel.CharacterCardRects)
            {
                # 在图像上绘制绿色矩形，表示角色卡片区域
                Cv2.Rectangle(bottomMat,
                    new Rect(rc.X, rc.Y - halfHeight, rc.Width, rc.Height), Scalar.Green, 1);
            }

            # 将处理后的图像保存到指定文件名
            Cv2.ImWrite(fileName, bottomMat);
        }
    }

    /// <summary>
    /// 通过OCR识别当前骰子数量
    /// </summary>
    /// <returns></returns>
    public int GetDiceCountByOcr()
    {
        # 捕获游戏灰度图像
        var srcMat = CaptureGameGreyMat();
        # 从灰度图像中裁剪出骰子数量区域
        var diceCountMap = new Mat(srcMat, _config.MyDiceCountRect);
        # 使用 OCR 识别骰子数量区域的文本
        var text = OcrFactory.Paddle.OcrWithoutDetector(diceCountMap);
        # 替换文本中的各种表示数字的字符
        text = text.Replace(" ", "")
            .Replace("①", "1")
            .Replace("②", "2")
            .Replace("③", "3")
            .Replace("④", "4")
            .Replace("⑤", "5")
            .Replace("⑥", "6")
            .Replace("⑦", "7")
            .Replace("⑧", "8")
            .Replace("⑨", "9")
            .Replace("⑩", "10")
            .Replace("⑪", "11")
            .Replace("⑫", "12")
            .Replace("⑬", "13")
            .Replace("⑭", "14")
            .Replace("⑮", "15");
        # 如果识别的文本为空或仅包含空白字符，记录警告日志
        if (string.IsNullOrWhiteSpace(text))
        {
            _logger.LogWarning("通过OCR识别当前骰子数量结果为空,无影响");
#if DEBUG
            # 如果是调试模式，将当前骰子计数为空的图像保存到日志目录
            Cv2.ImWrite($"log\\dice_count_empty{DateTime.Now:yyyy-MM-dd HH：mm：ss：ffff}.jpg", diceCountMap);
#endif
            # 返回 -10 表示骰子计数为空或出错
            return -10;
        }
        else if (RegexHelper.FullNumberRegex().IsMatch(text))
        {
            # 如果 OCR 识别的文本是有效数字，记录日志并返回该数字
            _logger.LogInformation("通过OCR识别当前骰子数量: {Text}", text);
            return int.Parse(text);
        }
        else
        {
            # 如果 OCR 识别的文本不是有效数字，记录警告日志
            _logger.LogWarning("通过OCR识别当前骰子结果: {Text}", text);
#if DEBUG
            # 如果是调试模式，将当前骰子计数错误的图像保存到日志目录
            Cv2.ImWrite($"log\\dice_count_error_{DateTime.Now:yyyy-MM-dd HH：mm：ss：ffff}.jpg", diceCountMap);
#endif
            # 返回 -10 表示骰子计数为空或出错
            return -10;
        }
    }
}
```