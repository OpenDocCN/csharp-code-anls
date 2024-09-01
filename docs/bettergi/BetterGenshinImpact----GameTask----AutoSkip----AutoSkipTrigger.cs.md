# `.\better-genshin-impact\BetterGenshinImpact\GameTask\AutoSkip\AutoSkipTrigger.cs`

```cs
# 引用 BetterGenshinImpact 项目的核心配置和识别相关命名空间
﻿using BetterGenshinImpact.Core.Config;
using BetterGenshinImpact.Core.Recognition;
using BetterGenshinImpact.Core.Recognition.OCR;
using BetterGenshinImpact.Core.Recognition.OpenCv;
using BetterGenshinImpact.Core.Simulator;
using BetterGenshinImpact.GameTask.AutoSkip.Assets;
using BetterGenshinImpact.GameTask.AutoSkip.Model;
using BetterGenshinImpact.GameTask.Common;
using BetterGenshinImpact.GameTask.Common.Element.Assets;
using BetterGenshinImpact.GameTask.Model.Area;
using BetterGenshinImpact.Helpers;
using BetterGenshinImpact.Service;
using BetterGenshinImpact.View.Drawable;
using Microsoft.Extensions.Logging;
using OpenCvSharp;
using System;
using System.Collections.Generic;
using System.Diagnostics;
using System.Linq;
using System.Text.Json;
using System.Text.RegularExpressions;
using System.Threading;
using System.Windows.Forms;
using Vanara.PInvoke;
using Region = BetterGenshinImpact.GameTask.Model.Area.Region;

# 声明命名空间 BetterGenshinImpact.GameTask.AutoSkip
namespace BetterGenshinImpact.GameTask.AutoSkip;

# 类的总结注释，说明此类用于自动剧情选项点击
/// <summary>
/// 自动剧情有选项点击
/// </summary>
public partial class AutoSkipTrigger : ITaskTrigger
{
    # 定义私有日志记录器，用于记录类的日志
    private readonly ILogger<AutoSkipTrigger> _logger = App.GetLogger<AutoSkipTrigger>();

    # 属性用于返回触发器的名称
    public string Name => "自动剧情";
    # 属性用于指示触发器是否启用
    public bool IsEnabled { get; set; }
    # 属性用于设置触发器的优先级
    public int Priority => 20;
    # 属性用于指示触发器是否独占资源
    public bool IsExclusive => false;

    # 属性用于指示触发器是否在后台运行
    public bool IsBackgroundRunning { get; set; }

    # 定义私有 AutoSkipAssets 实例
    private readonly AutoSkipAssets _autoSkipAssets;

    # 定义私有 AutoSkipConfig 配置实例
    private readonly AutoSkipConfig _config;

    # 私有字段用于存储不自动点击的选项列表，优先级低于橙色文字点击
    private List<string> _defaultPauseList = [];

    # 私有字段用于存储不自动点击的选项列表
    private List<string> _pauseList = [];

    # 私有字段用于存储优先自动点击的选项列表
    private List<string> _selectList = [];

    # 可空的 PostMessageSimulator 实例
    private PostMessageSimulator? _postMessageSimulator;

    # 构造函数，初始化 AutoSkipAssets 和 AutoSkipConfig 实例
    public AutoSkipTrigger()
    {
        _autoSkipAssets = AutoSkipAssets.Instance;
        _config = TaskContext.Instance().Config.AutoSkipConfig;
    }

    # 初始化方法的声明
    public void Init()
    # 从配置中读取是否启用标志
    IsEnabled = _config.Enabled;
    # 从配置中读取是否允许后台运行
    IsBackgroundRunning = _config.RunBackgroundEnabled;
    # 获取用于模拟消息发送的任务上下文实例
    _postMessageSimulator = TaskContext.Instance().PostMessageSimulator;

    try
    {
        # 尝试读取默认暂停选项的 JSON 文件内容
        var defaultPauseListJson = Global.ReadAllTextIfExist(@"User\AutoSkip\default_pause_options.json");
        # 如果文件内容不为空，则反序列化 JSON 内容为字符串列表
        if (!string.IsNullOrEmpty(defaultPauseListJson))
        {
            _defaultPauseList = JsonSerializer.Deserialize<List<string>>(defaultPauseListJson, ConfigService.JsonOptions) ?? [];
        }
    }
    catch (Exception e)
    {
        # 如果读取过程发生异常，记录错误日志
        _logger.LogError(e, "读取自动剧情默认暂停点击关键词列表失败");
        # 显示错误消息框
        MessageBox.Error("读取自动剧情默认暂停点击关键词列表失败，请确认修改后的自动剧情默认暂停点击关键词内容格式是否正确！");
    }

    try
    {
        # 尝试读取暂停选项的 JSON 文件内容
        var pauseListJson = Global.ReadAllTextIfExist(@"User\AutoSkip\pause_options.json");
        # 如果文件内容不为空，则反序列化 JSON 内容为字符串列表
        if (!string.IsNullOrEmpty(pauseListJson))
        {
            _pauseList = JsonSerializer.Deserialize<List<string>>(pauseListJson, ConfigService.JsonOptions) ?? [];
        }
    }
    catch (Exception e)
    {
        # 如果读取过程发生异常，记录错误日志
        _logger.LogError(e, "读取自动剧情暂停点击关键词列表失败");
        # 显示错误消息框
        MessageBox.Error("读取自动剧情暂停点击关键词列表失败，请确认修改后的自动剧情暂停点击关键词内容格式是否正确！");
    }

    try
    {
        # 尝试读取优先选择选项的 JSON 文件内容
        var selectListJson = Global.ReadAllTextIfExist(@"User\AutoSkip\select_options.json");
        # 如果文件内容不为空，则反序列化 JSON 内容为字符串列表
        if (!string.IsNullOrEmpty(selectListJson))
        {
            _selectList = JsonSerializer.Deserialize<List<string>>(selectListJson, ConfigService.JsonOptions) ?? [];
        }
    }
    catch (Exception e)
    {
        # 如果读取过程发生异常，记录错误日志
        _logger.LogError(e, "读取自动剧情优先点击选项列表失败");
        # 显示错误消息框
        MessageBox.Error("读取自动剧情优先点击选项列表失败，请确认修改后的自动剧情优先点击选项内容格式是否正确！");
    }

    /// <summary>
    /// 上一次播放中的帧
    /// </summary>
    private DateTime _prevPlayingTime = DateTime.MinValue;

    # 上一次执行的时间
    private DateTime _prevExecute = DateTime.MinValue;
    # 上一次闲聊执行的时间
    private DateTime _prevHangoutExecute = DateTime.MinValue;

    # 上一次获取每日奖励的时间
    private DateTime _prevGetDailyRewardsTime = DateTime.MinValue;

    # 上一次点击的时间
    private DateTime _prevClickTime = DateTime.MinValue;

    # 捕获内容的处理方法
    public void OnCapture(CaptureContent content)
    {
        # 如果距离上次执行时间小于等于 200 毫秒，则返回
        if ((DateTime.Now - _prevExecute).TotalMilliseconds <= 200)
        {
            return;
        }

        # 更新上次执行时间为当前时间
        _prevExecute = DateTime.Now;

        # 获取每日奖励
        GetDailyRewardsEsc(_config, content);

        # 使用内容的截图区域查找左上角的自动按钮
        using var foundRectArea = content.CaptureRectArea.Find(_autoSkipAssets.DisabledUiButtonRo);

        # 判断是否正在播放
        var isPlaying = !foundRectArea.IsEmpty(); 

        # 如果没有播放且距离上次播放时间小于 5 秒
        if (!isPlaying && (DateTime.Now - _prevPlayingTime).TotalSeconds <= 5)
        {
            # 关闭弹出页面
            ClosePopupPage(content);

            # 如果播放时间小于 3 秒，则尝试提交物品
            if ((DateTime.Now - _prevPlayingTime).TotalMilliseconds < 3000)
            {
                # 提交物品，如果成功，则返回
                if (SubmitGoods(content))
                {
                    return;
                }
            }
        }

        # 如果正在播放
        if (isPlaying)
        {
            # 更新上次播放时间为当前时间
            _prevPlayingTime = DateTime.Now;

            # 如果启用了快速跳过对话配置
            if (TaskContext.Instance().Config.AutoSkipConfig.QuicklySkipConversationsEnabled)
            {
                # 如果程序在后台运行
                if (IsBackgroundRunning)
                {
                    # 发送后台空格键模拟按键
                    _postMessageSimulator?.KeyPressBackground(User32.VK.VK_SPACE);
                }
                else
                {
                    # 发送前景空格键模拟按键
                    Simulation.SendInput.Keyboard.KeyPress(User32.VK.VK_SPACE);
                }
            }

            # 选择对话选项
            var hasOption = ChatOptionChoose(content.CaptureRectArea);

            # 如果启用了自动社交活动且没有选项可选
            if (_config.AutoHangoutEventEnabled && !hasOption)
            {
                # 如果距离上次社交活动时间小于 1.2 秒，则返回
                if ((DateTime.Now - _prevHangoutExecute).TotalMilliseconds < 1200)
                {
                    return;
                }

                # 更新上次社交活动时间为当前时间
                _prevHangoutExecute = DateTime.Now;

                # 选择社交活动选项
                HangoutOptionChoose(content.CaptureRectArea);
            }
        }
        else
        {
            # 点击黑屏游戏界面
            ClickBlackGameScreen(content);
        }
    }

    /// <summary>
    /// 黑屏点击判断
    /// </summary>
    /// <param name="content"></param>
    /// <returns></returns>
    private bool ClickBlackGameScreen(CaptureContent content)
    {
        // 黑屏剧情要点击鼠标（多次） 几乎全黑的时候不用点击
        // 如果距离上次点击时间超过 1200 毫秒
        if ((DateTime.Now - _prevClickTime).TotalMilliseconds > 1200)
        {
            // 从源灰度图像中提取上三分之一区域，并创建一个新的 Mat 对象
            using var grayMat = new Mat(content.CaptureRectArea.SrcGreyMat, new Rect(0, content.CaptureRectArea.SrcGreyMat.Height / 3, content.CaptureRectArea.SrcGreyMat.Width, content.CaptureRectArea.SrcGreyMat.Height / 3));
            // 计算灰度图像中颜色值为 0 的像素点数量
            var blackCount = OpenCvCommonHelper.CountGrayMatColor(grayMat, 0);
            // 计算黑色像素的比例
            var rate = blackCount * 1d / (grayMat.Width * grayMat.Height);
            // 如果黑色像素的比例在 0.5 到 0.98999 之间
            if (rate is >= 0.5 and < 0.98999)
            {
                // 如果背景运行中，模拟鼠标点击背景
                if (IsBackgroundRunning)
                {
                    TaskContext.Instance().PostMessageSimulator?.LeftButtonClickBackground();
                }
                // 否则直接模拟鼠标左键点击
                else
                {
                    Simulation.SendInput.Mouse.LeftButtonClick();
                }

                // 记录信息到日志中，表明进行了自动点击，及其比例
                _logger.LogInformation("自动剧情：{Text} 比例 {Rate}", "点击黑屏", rate.ToString("F"));

                // 更新上次点击时间
                _prevClickTime = DateTime.Now;
                return true;
            }
        }
        return false;
    }

    private void HangoutOptionChoose(ImageRegion captureRegion)
    }

    private bool IsOrangeOption(Mat textMat)
    {
        // 只提取橙色
        // Cv2.ImWrite($"log/text{DateTime.Now:yyyyMMddHHmmssffff}.png", textMat);
        // 对图像进行阈值处理，以提取橙色区域
        using var bMat = OpenCvCommonHelper.Threshold(textMat, new Scalar(243, 195, 48), new Scalar(255, 205, 55));
        // 计算图像中白色像素的数量
        var whiteCount = OpenCvCommonHelper.CountGrayMatColor(bMat, 255);
        // 计算白色像素的比例
        var rate = whiteCount * 1.0 / (bMat.Width * bMat.Height);
        // 输出识别到的橙色文字区域占比
        Debug.WriteLine($"识别到橙色文字区域占比:{rate}");
        // 如果橙色区域占比大于 0.06，则返回 true
        if (rate > 0.06)
        {
            return true;
        }

        return false;
    }

    /// <summary>
    /// 领取每日委托奖励 后 10s 寻找原石是否出现，出现则按下esc
    /// </summary>
    private void GetDailyRewardsEsc(AutoSkipConfig config, CaptureContent content)
    {
        // 如果配置中没有启用自动领取每日奖励，则直接返回
        if (!config.AutoGetDailyRewardsEnabled)
        {
            return;
        }

        // 如果距离上次领取奖励时间不超过 10 秒，则直接返回
        if ((DateTime.Now - _prevGetDailyRewardsTime).TotalSeconds > 10)
        {
            return;
        }

        // 在捕获区域中寻找原石图像
        content.CaptureRectArea.Find(_autoSkipAssets.PrimogemRo, primogemRa =>
        {
            // 等待 100 毫秒
            Thread.Sleep(100);
            // 模拟按下 ESC 键
            Simulation.SendInput.Keyboard.KeyPress(User32.VK.VK_ESCAPE);
            // 重置上次领取奖励时间
            _prevGetDailyRewardsTime = DateTime.MinValue;
            // 释放资源
            primogemRa.Dispose();
        });
    }

    [GeneratedRegex(@"^[a-zA-Z0-9]+$")]
    private static partial Regex EnOrNumRegex();

    /// <summary>
    /// 新的对话选项选择
    ///
    /// 返回 true 表示存在对话选项，但是不一定点击了
    /// </summary>
    private bool ChatOptionChoose(ImageRegion region)
    }

    private void ClickOcrRegion(Region region, string optionType = "")
    {
        # 如果 optionType 字符串为空或为 null，则等待配置指定的延迟时间
        if (string.IsNullOrEmpty(optionType))
        {
            Thread.Sleep(_config.AfterChooseOptionSleepDelay);
        }
        # 如果后台运行且原神游戏不活跃，则进行背景点击
        if (IsBackgroundRunning && !SystemControl.IsGenshinImpactActive())
        {
            region.BackgroundClick();
        }
        # 否则，进行普通点击
        else
        {
            region.Click();
        }
        # 自动跳过日志记录，传入区域文本
        AutoSkipLog(region.Text);
    }

    private void HangoutOptionClick(HangoutOption option)
    {
        # 如果配置的自动邀约选择选项的延迟时间大于0，则等待指定的时间
        if (_config.AutoHangoutChooseOptionSleepDelay > 0)
        {
            Thread.Sleep(_config.AutoHangoutChooseOptionSleepDelay);
        }
        # 如果后台运行且原神游戏不活跃，则对选项进行背景点击
        if (IsBackgroundRunning && !SystemControl.IsGenshinImpactActive())
        {
            option.BackgroundClick();
        }
        # 否则，进行普通点击
        else
        {
            option.Click();
        }
    }

    private void AutoHangoutSkipLog(string text)
    {
        # 如果当前时间与上次点击时间的间隔超过1000毫秒，则记录信息日志
        if ((DateTime.Now - _prevClickTime).TotalMilliseconds > 1000)
        {
            _logger.LogInformation("自动邀约：{Text}", text);
        }

        # 更新上次点击时间为当前时间
        _prevClickTime = DateTime.Now;
    }

    private void AutoSkipLog(string text)
    {
        # 如果文本包含“每日委托”或“探索派遣”，则记录信息日志
        if (text.Contains("每日委托") || text.Contains("探索派遣"))
        {
            _logger.LogInformation("自动剧情：{Text}", text);
        }
        # 否则，如果当前时间与上次点击时间的间隔超过1000毫秒，则记录信息日志
        else if ((DateTime.Now - _prevClickTime).TotalMilliseconds > 1000)
        {
            _logger.LogInformation("自动剧情：{Text}", text);
        }

        # 更新上次点击时间为当前时间
        _prevClickTime = DateTime.Now;
    }

    /// <summary>
    /// 关闭弹出页
    /// </summary>
    /// <param name="content"></param>
    private void ClosePopupPage(CaptureContent content)
    {
        # 如果配置中关闭弹出页的功能未启用，则直接返回
        if (!_config.ClosePopupPagedEnabled)
        {
            return;
        }

        # 在内容的捕获区域中查找自动跳过资产的“关闭”标记，并执行相应操作
        content.CaptureRectArea.Find(_autoSkipAssets.PageCloseRo, pageCloseRoRa =>
        {
            # 发送键盘 ESC 键以关闭弹出页
            TaskContext.Instance().PostMessageSimulator.KeyPress(User32.VK.VK_ESCAPE);

            # 自动跳过日志记录，标记为“关闭弹出页”
            AutoSkipLog("关闭弹出页");
            # 释放页面关闭标记资源
            pageCloseRoRa.Dispose();
        });
    }

    private bool SubmitGoods(CaptureContent content)
    }
能否提供更多的代码上下文？我需要了解这段代码是从哪里来的。
```