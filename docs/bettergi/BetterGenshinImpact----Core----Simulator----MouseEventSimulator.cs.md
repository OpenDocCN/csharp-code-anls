# `.\better-genshin-impact\BetterGenshinImpact\Core\Simulator\MouseEventSimulator.cs`

```cs
# 导入所需的命名空间和类
﻿using BetterGenshinImpact.Helpers;
using System.Threading;
using System.Windows;
using Vanara.PInvoke;

namespace BetterGenshinImpact.Core.Simulator;

# 定义一个模拟鼠标事件的类
public class MouseEventSimulator
{
    # 移动鼠标到指定的绝对坐标
    public void Move(int x, int y)
    {
        # 调用 Win32 API 的 mouse_event 函数，移动鼠标到指定的坐标
        User32.mouse_event(User32.MOUSEEVENTF.MOUSEEVENTF_ABSOLUTE | User32.MOUSEEVENTF.MOUSEEVENTF_MOVE,
            x * 65535 / PrimaryScreen.WorkingArea.Width, y * 65535 / PrimaryScreen.WorkingArea.Height,
            0, 0);
    }

    # 移动鼠标到指定的绝对坐标（无需缩放）
    public void MoveAbsolute(int x, int y)
    {
        # 调用 Win32 API 的 mouse_event 函数，直接移动鼠标到指定的坐标
        User32.mouse_event(User32.MOUSEEVENTF.MOUSEEVENTF_ABSOLUTE | User32.MOUSEEVENTF.MOUSEEVENTF_MOVE,
            x, y, 0, 0);
    }

    # 模拟鼠标左键按下事件
    public void LeftButtonDown()
    {
        # 调用 Win32 API 的 mouse_event 函数，模拟鼠标左键按下
        User32.mouse_event(User32.MOUSEEVENTF.MOUSEEVENTF_LEFTDOWN, 0, 0, 0, 0);
    }

    # 模拟鼠标左键抬起事件
    public void LeftButtonUp()
    {
        # 调用 Win32 API 的 mouse_event 函数，模拟鼠标左键抬起
        User32.mouse_event(User32.MOUSEEVENTF.MOUSEEVENTF_LEFTUP, 0, 0, 0, 0);
    }

    # 模拟点击操作
    public bool Click(int x, int y)
    {
        # 如果坐标为 (0, 0)，返回 false
        if (x == 0 && y == 0) return false;

        # 移动鼠标到指定坐标
        Move(x, y);
        # 模拟鼠标左键按下
        LeftButtonDown();
        # 等待 20 毫秒
        Thread.Sleep(20);
        # 模拟鼠标左键抬起
        LeftButtonUp();
        # 返回 true 表示点击成功
        return true;
    }

    # 使用 Point 对象进行点击操作
    public bool Click(Point point)
    {
        # 将 Point 对象的坐标转换为整数，并调用 Click 方法
        return Click((int)point.X, (int)point.Y);
    }

    # 模拟双击操作
    public bool DoubleClick(Point point)
    {
        # 执行一次点击操作
        Click(point);
        # 等待 200 毫秒
        Thread.Sleep(200);
        # 执行第二次点击操作
        return Click(point);
    }
}
```