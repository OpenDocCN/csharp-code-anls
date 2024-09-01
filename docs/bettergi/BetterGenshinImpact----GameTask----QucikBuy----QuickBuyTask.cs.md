# `.\better-genshin-impact\BetterGenshinImpact\GameTask\QucikBuy\QuickBuyTask.cs`

```cs
﻿using BetterGenshinImpact.Core.Simulator; // 引用 BetterGenshinImpact.Core.Simulator 命名空间
using BetterGenshinImpact.GameTask.Common; // 引用 BetterGenshinImpact.GameTask.Common 命名空间
using BetterGenshinImpact.GameTask.Model.Area; // 引用 BetterGenshinImpact.GameTask.Model.Area 命名空间
using Microsoft.Extensions.Logging; // 引用 Microsoft.Extensions.Logging 命名空间
using System; // 引用 System 命名空间
using Wpf.Ui.Violeta.Controls; // 引用 Wpf.Ui.Violeta.Controls 命名空间

namespace BetterGenshinImpact.GameTask.QucikBuy; // 定义命名空间 BetterGenshinImpact.GameTask.QucikBuy

public class QuickBuyTask // 定义 QuickBuyTask 类
{
    public static void Done() // 定义静态方法 Done
    {
        if (!TaskContext.Instance().IsInitialized) // 检查 TaskContext 实例是否已初始化
        {
            Toast.Warning("请先启动"); // 弹出警告提示，要求用户先启动
            return; // 退出方法
        }
        if (!SystemControl.IsGenshinImpactActiveByProcess()) // 检查 Genshin Impact 是否在运行
        {
            return; // 如果没有运行，则退出方法
        }

        try // 尝试执行以下代码块
        {
            // 点击购买/兑换区域，位置为右下角，坐标计算为 (size.Width - 225 * scale, size.Height - 60 * scale)
            GameCaptureRegion.GameRegionClick((size, scale) => (size.Width - 225 * scale, size.Height - 60 * scale));
            TaskControl.CheckAndSleep(100); // 等待窗口弹出，暂停100毫秒

            // 移动到左边点，位置为 742x601
            GameCaptureRegion.GameRegion1080PPosMove(742, 601);
            TaskControl.CheckAndSleep(100); // 等待操作完成，暂停100毫秒
            Simulation.SendInput.Mouse.LeftButtonDown(); // 按下鼠标左键
            TaskControl.CheckAndSleep(50); // 等待操作完成，暂停50毫秒

            // 向右滑动，移动距离为 (1000, 0)
            Simulation.SendInput.Mouse.MoveMouseBy(1000, 0);
            TaskControl.CheckAndSleep(200); // 等待滑动完成，暂停200毫秒
            Simulation.SendInput.Mouse.LeftButtonUp(); // 释放鼠标左键
            TaskControl.CheckAndSleep(100); // 等待操作完成，暂停100毫秒

            // 点击弹出页的购买/兑换区域，位置为 1100x780
            GameCaptureRegion.GameRegion1080PPosClick(1100, 780);
            TaskControl.CheckAndSleep(200); // 等待窗口消失，暂停200毫秒
            // 再次点击购买/兑换区域，位置为右下角，坐标计算为 (size.Width - 225 * scale, size.Height - 60 * scale)
            GameCaptureRegion.GameRegionClick((size, scale) => (size.Width - 225 * scale, size.Height - 60 * scale));
            TaskControl.CheckAndSleep(200); // 等待操作完成，暂停200毫秒
        }
        catch (Exception e) // 捕获并处理异常
        {
            TaskControl.Logger.LogWarning(e.Message); // 记录警告日志，输出异常消息
        }
    }
}
```