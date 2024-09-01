# `.\better-genshin-impact\BetterGenshinImpact\GameTask\AutoWood\AutoWoodTask.cs`

```cs
# 引入必要的命名空间
﻿using BetterGenshinImpact.Core.Recognition.OCR;
using BetterGenshinImpact.Core.Simulator;
using BetterGenshinImpact.GameTask.AutoGeniusInvokation.Exception;
using BetterGenshinImpact.GameTask.AutoWood.Assets;
using BetterGenshinImpact.GameTask.AutoWood.Utils;
using BetterGenshinImpact.GameTask.Common;
using BetterGenshinImpact.GameTask.Model.Area;
using BetterGenshinImpact.Genshin.Settings;
using BetterGenshinImpact.View.Drawable;
using BetterGenshinImpact.ViewModel.Pages;
using Microsoft.Extensions.Logging;
using System;
using System.Collections.Concurrent;
using System.Collections.Generic;
using System.Diagnostics;
using System.Linq;
using System.Text.RegularExpressions;
using Vanara.PInvoke;
using static BetterGenshinImpact.GameTask.Common.TaskControl;
using static Vanara.PInvoke.User32;
using GC = System.GC;

namespace BetterGenshinImpact.GameTask.AutoWood;

/// <summary>
/// 自动伐木
/// </summary>
public partial class AutoWoodTask
{
    # 私有只读字段，保存资源对象
    private readonly AutoWoodAssets _assets;

    # 私有字段，标记是否为第一次操作
    private bool _first = true;

    # 私有只读字段，打印伐木统计数据的工具
    private readonly WoodStatisticsPrinter _printer;

    # 私有只读字段，第三方登录对象
    private readonly Login3rdParty _login3rdParty;

    # 私有字段，用于模拟按键操作
    private VK _zKey = VK.VK_Z;

    # 构造函数，初始化对象
    public AutoWoodTask()
    {
        _login3rdParty = new();
        AutoWoodAssets.DestroyInstance();
        _assets = AutoWoodAssets.Instance;
        _printer = new WoodStatisticsPrinter(_assets);
    }

    # 开始执行伐木任务
    public void Start(WoodTaskParam taskParam)
    }

    # 内部类，用于打印伐木统计信息
    private partial class WoodStatisticsPrinter(AutoWoodAssets assert)
    }

    # 执行伐木操作
    private void Felling(WoodTaskParam taskParam, bool isLast = false)
    {
        # 1. 按 z 触发「王树瑞佑」
        PressZ(taskParam);

        # 如果这是最后一次操作，直接返回
        if (isLast)
        {
            return;
        }

        # 打印伐木的统计数据（可选）
        if (TaskContext.Instance().Config.AutoWoodConfig.WoodCountOcrEnabled)
        {
            _printer.PrintWoodStatistics(taskParam);
            # 如果统计信息为空或达到了最大木材数量，则返回
            if (_printer.WoodStatisticsAlwaysEmpty() || _printer.ReachedWoodMaxCount) return;
        }

        # 2. 按下 ESC 打开菜单 并退出游戏
        PressEsc(taskParam);

        # 3. 等待进入游戏
        EnterGame(taskParam);

        # 手动触发垃圾回收
        GC.Collect();
    }

    # 模拟按 Z 键操作
    private void PressZ(WoodTaskParam taskParam)
    {
        # 重要：在按 Z 键之前必须尝试聚焦
        SystemControl.Focus(TaskContext.Instance().GameHandle);

        # 如果是第一次操作，进行初始化检查
        if (_first)
        {
            using var contentRegion = CaptureToRectArea();
            using var ra = contentRegion.Find(_assets.TheBoonOfTheElderTreeRo);
            # 如果找不到「王树瑞佑」，抛出异常或模拟按键
            if (ra.IsEmpty())
            {
#if !TEST_WITHOUT_Z_ITEM
                throw new NormalEndException("请先装备小道具「王树瑞佑」！");
#else
                System.Threading.Thread.Sleep(2000);
                Simulation.SendInput.Keyboard.KeyPress(_zKey);
                Debug.WriteLine("[AutoWood] Z");
                _first = false;
#endif
            }
            else
            {
                // 模拟按下键盘上的 Z 键
                Simulation.SendInput.Keyboard.KeyPress(_zKey);
                // 输出调试信息，表示 Z 键被按下
                Debug.WriteLine("[AutoWood] Z");
                // 将 _first 设置为 false
                _first = false;
            }
        }
        else
        {
            // 尝试执行以下操作，最多重试 120 次，每次间隔 1 秒
            NewRetry.Do(() =>
            {
                // 暂停 1 毫秒
                Sleep(1, taskParam.Cts);
                // 捕获当前区域的内容
                using var contentRegion = CaptureToRectArea();
                // 在捕获的区域中查找特定资源
                using var ra = contentRegion.Find(_assets.TheBoonOfTheElderTreeRo);
                // 如果未找到资源
                if (ra.IsEmpty())
                {
#if !TEST_WITHOUT_Z_ITEM
                    // 抛出异常以指示未找到指定的资源
                    throw new RetryException("未找到「王树瑞佑」");
#else
                    // 如果在测试模式中，暂停 15 秒
                    System.Threading.Thread.Sleep(15000);
#endif
                }

                // 模拟按下键盘上的 Z 键
                Simulation.SendInput.Keyboard.KeyPress(_zKey);
                // 输出调试信息，表示 Z 键被按下
                Debug.WriteLine("[AutoWood] Z");
                // 暂停 500 毫秒
                Sleep(500, taskParam.Cts);
            }, TimeSpan.FromSeconds(1), 120);
        }

        // 暂停 300 毫秒
        Sleep(300, taskParam.Cts);
        // 暂停指定的延迟时间
        Sleep(TaskContext.Instance().Config.AutoWoodConfig.AfterZSleepDelay, taskParam.Cts);
    }

    private void PressEsc(WoodTaskParam taskParam)
    {
        // 将焦点切换到游戏窗口
        SystemControl.Focus(TaskContext.Instance().GameHandle);
        // 模拟按下 ESC 键
        Simulation.SendInput.Keyboard.KeyPress(VK.VK_ESCAPE);
        // 如果启用了双 ESC 模式（注释掉的代码）
        // if (TaskContext.Instance().Config.AutoWoodConfig.PressTwoEscEnabled)
        // {
        //     // 暂停 1500 毫秒
        //     Sleep(1500, taskParam.Cts);
        //     // 模拟按下 ESC 键
        //     Simulation.SendInput.Keyboard.KeyPress(VK.VK_ESCAPE);
        // }
        // 输出调试信息，表示 ESC 键被按下
        Debug.WriteLine("[AutoWood] Esc");
        // 暂停 800 毫秒
        Sleep(800, taskParam.Cts);
        // 确认是否在菜单界面
        try
        {
            // 尝试执行以下操作，最多重试 5 次，每次间隔 1.2 秒
            NewRetry.Do(() =>
            {
                // 暂停 1 毫秒
                Sleep(1, taskParam.Cts);
                // 捕获当前区域的内容
                using var contentRegion = CaptureToRectArea();
                // 在捕获的区域中查找菜单包
                using var ra = contentRegion.Find(_assets.MenuBagRo);
                // 如果未找到菜单包
                if (ra.IsEmpty())
                {
                    // 模拟按下 ESC 键
                    Simulation.SendInput.Keyboard.KeyPress(VK.VK_ESCAPE);
                    // 抛出异常以指示未检测到弹出菜单
                    throw new RetryException("未检测到弹出菜单");
                }
            }, TimeSpan.FromSeconds(1.2), 5);
        }
        catch (Exception e)
        {
            // 记录异常信息
            Logger.LogInformation(e.Message);
            // 记录“仍旧点击退出按钮”的信息
            Logger.LogInformation("仍旧点击退出按钮");
        }

        // 点击退出按钮
        GameCaptureRegion.GameRegionClick((size, scale) => (50 * scale, size.Height - 50 * scale));

        // 输出调试信息，表示点击了退出按钮
        Debug.WriteLine("[AutoWood] Click exit button");

        // 暂停 500 毫秒
        Sleep(500, taskParam.Cts);

        // 捕获当前区域的内容
        using var contentRegion = CaptureToRectArea();
        // 在捕获的区域中查找确认按钮
        contentRegion.Find(_assets.ConfirmRo, ra =>
        {
            // 点击确认按钮
            ra.Click();
            // 输出调试信息，表示点击了确认按钮
            Debug.WriteLine("[AutoWood] Click confirm button");
            // 释放资源
            ra.Dispose();
        });
    }

    private void EnterGame(WoodTaskParam taskParam)
    # 如果第三方登录可用
    if (_login3rdParty.IsAvailabled)
    {
        # 睡眠 1 毫秒，传递取消令牌
        Sleep(1, taskParam.Cts);
        # 执行第三方登录操作，传递取消令牌
        _login3rdParty.Login(taskParam.Cts);
    }

    # 初始化点击计数器
    var clickCnt = 0;
    # 执行最多 50 次循环
    for (var i = 0; i < 50; i++)
    {
        # 每次循环睡眠 1 毫秒，传递取消令牌
        Sleep(1, taskParam.Cts);

        # 捕获屏幕指定区域的内容
        using var contentRegion = CaptureToRectArea();
        # 查找图像中是否包含特定资源
        using var ra = contentRegion.Find(_assets.EnterGameRo);
        # 如果找到目标区域
        if (!ra.IsEmpty())
        {
            # 点击计数器加 1
            clickCnt++;
            # 在指定位置点击
            GameCaptureRegion.GameRegion1080PPosClick(955, 666);
            # 输出调试信息
            Debug.WriteLine("[AutoWood] Click entry");
        }
        else
        {
            # 如果点击次数超过 2 次
            if (clickCnt > 2)
            {
                # 睡眠 5 秒，传递取消令牌
                Sleep(5000, taskParam.Cts);
                # 退出循环
                break;
            }
        }

        # 每次循环睡眠 1 秒，传递取消令牌
        Sleep(1000, taskParam.Cts);
    }

    # 如果点击计数器为 0，抛出重试异常
    if (clickCnt == 0)
    {
        throw new RetryException("未检测进入游戏界面");
    }
# 代码块的结束符号，标记代码的结束
}
```