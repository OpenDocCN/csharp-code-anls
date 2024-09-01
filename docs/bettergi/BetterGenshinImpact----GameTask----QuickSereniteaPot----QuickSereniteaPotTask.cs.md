# `.\better-genshin-impact\BetterGenshinImpact\GameTask\QuickSereniteaPot\QuickSereniteaPotTask.cs`

```cs
# 引入所需的命名空间和类
﻿using BetterGenshinImpact.Core.Simulator;
using BetterGenshinImpact.GameTask.AutoGeniusInvokation.Exception;
using BetterGenshinImpact.GameTask.Common;
using BetterGenshinImpact.GameTask.Model.Area;
using BetterGenshinImpact.GameTask.QuickSereniteaPot.Assets;
using BetterGenshinImpact.View.Drawable;
using Microsoft.Extensions.Logging;
using System;
using Wpf.Ui.Violeta.Controls;
using static Vanara.PInvoke.User32;

# 定义命名空间
namespace BetterGenshinImpact.GameTask.QuickSereniteaPot;

# 定义 QuickSereniteaPotTask 类
public class QuickSereniteaPotTask
{
    # 定义一个私有静态方法，用于等待背包打开
    private static void WaitForBagToOpen()
    {
        # 使用重试机制执行操作，直到成功或达到最大重试次数
        NewRetry.Do(() =>
        {
            # 休眠1秒，等待界面更新
            TaskControl.Sleep(1);
            # 捕获当前区域，并查找背包关闭按钮图标
            using var ra2 = TaskControl.CaptureToRectArea().Find(QuickSereniteaPotAssets.Instance.BagCloseButtonRo);
            # 如果未找到背包关闭按钮，则抛出异常
            if (ra2.IsEmpty())
            {
                throw new RetryException("背包未打开");
            }
        }, TimeSpan.FromMilliseconds(500), 3);
    }

    # 定义一个私有静态方法，用于查找并点击壶图标
    private static void FindPotIcon()
    {
        # 使用重试机制执行操作，直到成功或达到最大重试次数
        NewRetry.Do(() =>
        {
            # 休眠1秒，等待界面更新
            TaskControl.Sleep(1);
            # 捕获当前区域，并查找壶图标
            using var ra2 = TaskControl.CaptureToRectArea().Find(QuickSereniteaPotAssets.Instance.SereniteaPotIconRo);
            # 如果未找到壶图标，则抛出异常
            if (ra2.IsEmpty())
            {
                throw new RetryException("未检测到壶");
            }
            # 如果找到壶图标，则点击它
            else
            {
                ra2.Click();
            }
        }, TimeSpan.FromMilliseconds(200), 3);
    }

    # 定义一个公共静态方法，执行任务
    public static void Done()
    {
        # 如果任务上下文未初始化，则弹出警告
        if (!TaskContext.Instance().IsInitialized)
        {
            Toast.Warning("请先启动");
            return;
        }

        # 如果原神进程未激活，则直接返回
        if (!SystemControl.IsGenshinImpactActiveByProcess())
        {
            return;
        }

        # 销毁 QuickSereniteaPotAssets 实例
        QuickSereniteaPotAssets.DestroyInstance();

        try
        {
            # 模拟按下 B 键以打开背包
            Simulation.SendInput.Keyboard.KeyPress(VK.VK_B);
            # 等待背包打开
            TaskControl.CheckAndSleep(500);
            WaitForBagToOpen();

            # 点击道具页的指定位置
            GameCaptureRegion.GameRegion1080PPosClick(1050, 50);
            # 等待点击完成
            TaskControl.CheckAndSleep(200);

            # 尝试找到并点击壶图标
            FindPotIcon();
            # 等待操作完成
            TaskControl.CheckAndSleep(200);

            # 点击放置按钮，坐标为右下角的调整值
            GameCaptureRegion.GameRegionClick((size, assetScale) => (size.Width - 225 * assetScale, size.Height - 60 * assetScale));
            # 也可以使用以下方法点击放置按钮
            # Bv.ClickWhiteConfirmButton(TaskControl.CaptureToRectArea());
            # 等待操作完成
            TaskControl.CheckAndSleep(800);

            # 模拟按下 F 键
            Simulation.SendInput.Keyboard.KeyPress(VK.VK_F);
        }
        catch (Exception e)
        {
            # 记录异常警告
            TaskControl.Logger.LogWarning(e.Message);
        }
        finally
        {
            # 清除视图内容
            VisionContext.Instance().DrawContent.ClearAll();
        }
    }
}
```