# `.\better-genshin-impact\BetterGenshinImpact\GameTask\AutoPick\AutoPickTrigger.cs`

```cs
# 引入 BetterGenshinImpact 核心配置相关的命名空间
﻿using BetterGenshinImpact.Core.Config;
# 引入 BetterGenshinImpact 核心识别相关的命名空间
using BetterGenshinImpact.Core.Recognition;
# 引入 OCR 识别相关的命名空间
using BetterGenshinImpact.Core.Recognition.OCR;
# 引入 ONNX SVTR 识别相关的命名空间
using BetterGenshinImpact.Core.Recognition.ONNX.SVTR;
# 引入 OpenCV 识别相关的命名空间
using BetterGenshinImpact.Core.Recognition.OpenCv;
# 引入定时器配置相关的命名空间
using BetterGenshinImpact.Core.Script.Dependence.Model.TimerConfig;
# 引入模拟器相关的命名空间
using BetterGenshinImpact.Core.Simulator;
# 引入自动拾取任务相关的资源命名空间
using BetterGenshinImpact.GameTask.AutoPick.Assets;
# 引入辅助功能的命名空间
using BetterGenshinImpact.Helpers;
# 引入服务相关的命名空间
using BetterGenshinImpact.Service;
# 引入日志记录相关的命名空间
using Microsoft.Extensions.Logging;
# 引入 OpenCVSharp 相关的命名空间
using OpenCvSharp;
# 引入系统基本功能相关的命名空间
using System;
# 引入集合相关的命名空间
using System.Collections.Generic;
# 引入诊断工具相关的命名空间
using System.Diagnostics;
# 引入 JSON 处理相关的命名空间
using System.Text.Json;
# 引入正则表达式处理相关的命名空间
using System.Text.RegularExpressions;
# 引入 Vanara PInvoke 相关的命名空间
using Vanara.PInvoke;

# 定义命名空间 BetterGenshinImpact.GameTask.AutoPick
namespace BetterGenshinImpact.GameTask.AutoPick;

# 定义类 AutoPickTrigger，实现 ITaskTrigger 接口
public partial class AutoPickTrigger : ITaskTrigger
{
    # 定义私有只读字段 _logger，用于日志记录
    private readonly ILogger<AutoPickTrigger> _logger = App.GetLogger<AutoPickTrigger>();
    # 定义私有只读字段 _pickTextInference，用于文本推断
    private readonly ITextInference _pickTextInference = TextInferenceFactory.Pick;

    # 实现 ITaskTrigger 接口的 Name 属性，返回任务名称
    public string Name => "自动拾取";
    # 实现 ITaskTrigger 接口的 IsEnabled 属性，表示任务是否启用
    public bool IsEnabled { get; set; }
    # 实现 ITaskTrigger 接口的 Priority 属性，定义任务优先级
    public int Priority => 30;
    # 实现 ITaskTrigger 接口的 IsExclusive 属性，表示任务是否为独占任务
    public bool IsExclusive => false;

    # 定义私有只读字段 _autoPickAssets，用于获取自动拾取的资源
    private readonly AutoPickAssets _autoPickAssets;

    # 定义私有字段 _blackList，表示拾取黑名单
    private List<string> _blackList = [];

    # 定义私有字段 _whiteList，表示拾取白名单
    private List<string> _whiteList = [];

    # 定义私有字段 _pickKeyName，表示自定义拾取按键
    private string _pickKeyName = "F";

    # 定义私有字段 _pickVk，表示自定义拾取按键的虚拟键码
    private User32.VK _pickVk = User32.VK.VK_F;
    # 定义私有字段 _pickRo，表示识别对象
    private RecognitionObject _pickRo;

    # 定义私有字段 _externalConfig，表示外部配置
    private AutoPickExternalConfig? _externalConfig;

    # 默认构造函数
    public AutoPickTrigger()
    {
        # 初始化 _autoPickAssets 字段为 AutoPickAssets 的单例实例
        _autoPickAssets = AutoPickAssets.Instance;
        # 初始化 _pickRo 字段为 _autoPickAssets 的 FRo 属性
        _pickRo = _autoPickAssets.FRo;
    }

    # 带外部配置的构造函数，调用默认构造函数并初始化外部配置
    public AutoPickTrigger(AutoPickExternalConfig? config) : this()
    {
        # 将外部配置赋值给 _externalConfig 字段
        _externalConfig = config;
    }

    # 初始化方法
    public void Init()
    {
        // 从任务上下文中获取自动拾取配置中的拾取键
        var keyName = TaskContext.Instance().Config.AutoPickConfig.PickKey;
        // 如果拾取键不为空
        if (!string.IsNullOrEmpty(keyName))
        {
            try
            {
                // 加载自定义拾取键
                _pickRo = _autoPickAssets.LoadCustomPickKey(keyName);
                // 将拾取键转换为虚拟键码
                _pickVk = User32Helper.ToVk(keyName);
                // 保存拾取键名称
                _pickKeyName = keyName;
            }
            catch (Exception e)
            {
                // 记录加载自定义拾取按键异常的调试日志
                _logger.LogDebug(e, "加载自定义拾取按键时发生异常");
                // 记录加载失败的错误日志，并设置为默认的F键
                _logger.LogError("加载自定义拾取按键失败，继续使用默认的F键");
                TaskContext.Instance().Config.AutoPickConfig.PickKey = "F";
                return;
            }
            // 如果自定义拾取键不是F键，则记录信息日志
            if (_pickKeyName != "F")
            {
                _logger.LogInformation("自定义拾取按键：{Key}", _pickKeyName);
            }
        }

        // 从任务上下文中获取自动拾取配置的启用状态
        IsEnabled = TaskContext.Instance().Config.AutoPickConfig.Enabled;
        try
        {
            // 尝试读取黑名单 JSON 文件
            var blackListJson = Global.ReadAllTextIfExist(@"User\pick_black_lists.json");
            // 如果黑名单 JSON 不为空，则反序列化为字符串列表
            if (!string.IsNullOrEmpty(blackListJson))
            {
                _blackList = JsonSerializer.Deserialize<List<string>>(blackListJson, ConfigService.JsonOptions) ?? [];
            }
        }
        catch (Exception e)
        {
            // 记录读取黑名单失败的错误日志
            _logger.LogError(e, "读取拾取黑名单失败");
            // 显示读取黑名单失败的错误消息框
            MessageBox.Error("读取拾取黑名单失败，请确认修改后的拾取黑名单内容格式是否正确！");
        }

        try
        {
            // 尝试读取白名单 JSON 文件
            var whiteListJson = Global.ReadAllTextIfExist(@"User\pick_white_lists.json");
            // 如果白名单 JSON 不为空，则反序列化为字符串列表
            if (!string.IsNullOrEmpty(whiteListJson))
            {
                _whiteList = JsonSerializer.Deserialize<List<string>>(whiteListJson, ConfigService.JsonOptions) ?? [];
            }
        }
        catch (Exception e)
        {
            // 记录读取白名单失败的错误日志
            _logger.LogError(e, "读取拾取白名单失败");
            // 显示读取白名单失败的错误消息框
            MessageBox.Error("读取拾取白名单失败，请确认修改后的拾取白名单内容格式是否正确！");
        }
    }

    /// <summary>
    /// 用于日志只输出一次
    /// </summary>
    private string _lastText = string.Empty;

    /// <summary>
    /// 用于日志只输出一次
    /// </summary>
    private int _prevClickFrameIndex = -1;

    //private int _fastModePickCount = 0;

    public void OnCapture(CaptureContent content)
    }

    private Mat PreProcessForInference(Mat mat)
    {
        // Yap 已经改用灰度图了 https://github.com/Alex-Beng/Yap/commit/c2ad1e7b1442aaf2d80782a032e00876cd1c6c84
        // 二值化
        // Cv2.Threshold(mat, mat, 0, 255, ThresholdTypes.Otsu | ThresholdTypes.Binary);
        //Cv2.AdaptiveThreshold(mat, mat, 255, AdaptiveThresholdTypes.GaussianC, ThresholdTypes.Binary, 31, 3); // 效果不错 但是和模型不搭
        //mat = OpenCvCommonHelper.Threshold(mat, Scalar.FromRgb(235, 235, 235), Scalar.FromRgb(255, 255, 255)); // 识别物品不太行
        // 不知道为什么要强制拉伸到 221x32
        mat = ResizeHelper.ResizeTo(mat, 221, 32);
        // 填充到 384x32
        var padded = new Mat(new Size(384, 32), MatType.CV_8UC1, Scalar.Black);
        padded[new Rect(0, 0, mat.Width, mat.Height)] = mat;
        //Cv2.ImWrite(Global.Absolute("padded.png"), padded);
        return padded;
    }

    /// <summary>
    /// 只在相同文字前后3帧内输出一次
    /// </summary>
    /// <param name="content"></param>
    /// <param name="text"></param>
    private void LogPick(CaptureContent content, string text)
    {
        // 如果当前文字与上次记录的文字不同，或者当前文字相同但前后帧数差距大于等于5，则记录日志
        if (_lastText != text || (_lastText == text && Math.Abs(content.FrameIndex - _prevClickFrameIndex) >= 5))
        {
            _logger.LogInformation("交互或拾取：{Text}", text);
        }

        // 更新记录的文字为当前文字
        _lastText = text;
        // 更新前一个点击的帧索引为当前帧索引
        _prevClickFrameIndex = content.FrameIndex;
    }

    // 生成一个正则表达式，用于匹配开头或结尾的标点符号和空白字符
    [GeneratedRegex(@"^[\p{P} ]+|[\p{P} ]+$")]
    private static partial Regex PunctuationAndSpacesRegex();
# 结束当前代码块或结构的标志
}
```