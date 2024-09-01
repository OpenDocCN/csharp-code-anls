# `.\better-genshin-impact\BetterGenshinImpact\GameTask\Model\Area\DesktopRegion.cs`

```cs
﻿using BetterGenshinImpact.Core.Simulator; // 引入 BetterGenshinImpact.Core.Simulator 命名空间
using BetterGenshinImpact.GameTask.Model.Area.Converter; // 引入 BetterGenshinImpact.GameTask.Model.Area.Converter 命名空间
using BetterGenshinImpact.Helpers; // 引入 BetterGenshinImpact.Helpers 命名空间
using System.Drawing; // 引入 System.Drawing 命名空间，用于处理图形相关的操作

namespace BetterGenshinImpact.GameTask.Model.Area; // 定义 BetterGenshinImpact.GameTask.Model.Area 命名空间

/// <summary>
/// 桌面区域类
/// 无缩放的桌面屏幕大小
/// 主要用于点击操作
/// </summary>
public class DesktopRegion() : Region(0, 0, PrimaryScreen.WorkingArea.Width, PrimaryScreen.WorkingArea.Height)
{
    // 构造函数，调用父类 Region 的构造函数，初始化桌面区域的大小
    public void DesktopRegionClick(int x, int y, int w, int h)
    {
        // 计算鼠标点击位置的中点，并将其移动到相应位置，模拟鼠标点击操作
        Simulation.SendInput.Mouse.MoveMouseTo((x + (w * 1d / 2)) * 65535 / Width,
            (y + (h * 1d / 2)) * 65535 / Height).LeftButtonClick().Sleep(50).LeftButtonUp();
    }

    public void DesktopRegionMove(int x, int y, int w, int h)
    {
        // 计算鼠标移动位置的中点，并将其移动到相应位置，模拟鼠标移动操作
        Simulation.SendInput.Mouse.MoveMouseTo((x + (w * 1d / 2)) * 65535 / Width,
            (y + (h * 1d / 2)) * 65535 / Height);
    }

    /// <summary>
    /// 静态方法,每次都会重新计算屏幕大小
    /// </summary>
    /// <param name="cx"></param>
    /// <param name="cy"></param>
    public static void DesktopRegionClick(double cx, double cy)
    {
        // 计算鼠标点击位置，并将其移动到相应位置，模拟鼠标点击操作
        Simulation.SendInput.Mouse.MoveMouseTo(cx * 65535 * 1d / PrimaryScreen.WorkingArea.Width,
            cy * 65535 * 1d / PrimaryScreen.WorkingArea.Height).LeftButtonDown().Sleep(50).LeftButtonUp();
    }

    public static void DesktopRegionMove(double cx, double cy)
    {
        // 计算鼠标移动位置，并将其移动到相应位置，模拟鼠标移动操作
        Simulation.SendInput.Mouse.MoveMouseTo(cx * 65535 * 1d / PrimaryScreen.WorkingArea.Width,
            cy * 65535 * 1d / PrimaryScreen.WorkingArea.Height);
    }

    public GameCaptureRegion Derive(Bitmap captureBitmap, int x, int y)
    {
        // 创建 GameCaptureRegion 实例，用于捕获屏幕区域并应用转换器
        return new GameCaptureRegion(captureBitmap, x, y, this, new TranslationConverter(x, y));
    }
}
```