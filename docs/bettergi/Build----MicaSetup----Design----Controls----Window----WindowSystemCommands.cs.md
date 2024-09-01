# `.\better-genshin-impact\Build\MicaSetup\Design\Controls\Window\WindowSystemCommands.cs`

```cs
# 引入必要的命名空间和类
﻿using MicaSetup.Helper;
using MicaSetup.Natives;
using System.Security;
using System.Windows;
using System.Windows.Interop;

namespace MicaSetup.Design.Controls;

# 定义一个内部静态类，用于处理窗口系统命令
internal static class WindowSystemCommands
{
    # 标记方法为安全关键方法，允许对受保护资源进行操作
    [SecuritySafeCritical]
    # 定义一个公共静态方法，用于显示系统菜单
    public static void ShowSystemMenu(Window window, Point? screenLocation = null!)
    {
        # 如果没有提供屏幕位置
        if (screenLocation == null)
        {
            # 获取当前鼠标光标的位置
            if (User32.GetCursorPos(out POINT pt))
            {
                # 将光标位置转换为 DPI 缩放后的点，并显示系统菜单
                SystemCommands.ShowSystemMenu(window, new Point(DpiHelper.CalcDPiX(pt.X), DpiHelper.CalcDPiY(pt.Y)));
            }
        }
        # 如果提供了屏幕位置
        else
        {
            # 将提供的屏幕位置转换为 DPI 缩放后的点，并显示系统菜单
            SystemCommands.ShowSystemMenu(window, new Point(DpiHelper.CalcDPiX(screenLocation.Value.X), DpiHelper.CalcDPiY(screenLocation.Value.Y)));
        }
    }

    # 标记方法为安全关键方法
    [SecuritySafeCritical]
    # 定义一个公共静态方法，用于关闭窗口
    public static void CloseWindow(Window window)
    {
        # 调用系统命令关闭窗口
        SystemCommands.CloseWindow(window);
    }

    # 标记方法为安全关键方法
    [SecuritySafeCritical]
    # 定义一个公共静态方法，用于最大化窗口
    public static void MaximizeWindow(Window window)
    {
        # 调用系统命令最大化窗口
        SystemCommands.MaximizeWindow(window);
    }

    # 标记方法为安全关键方法
    [SecuritySafeCritical]
    # 定义一个公共静态方法，用于最小化窗口
    public static void MinimizeWindow(Window window)
    {
        # 调用系统命令最小化窗口
        SystemCommands.MinimizeWindow(window);
    }

    # 标记方法为安全关键方法
    [SecuritySafeCritical]
    # 定义一个公共静态方法，用于恢复窗口（从最大化或最小化状态）
    public static void RestoreWindow(Window window)
    {
        # 调用系统命令恢复窗口到正常状态
        SystemCommands.RestoreWindow(window);
        # 将窗口样式设置为单边框窗口
        window.WindowStyle = WindowStyle.SingleBorderWindow;
    }

    # 标记方法为安全关键方法
    [SecuritySafeCritical]
    # 定义一个公共静态方法，用于快速恢复窗口
    public static void FastRestoreWindow(Window window, bool force = false)
    {
        # 发送消息以模拟鼠标左键按下事件，来恢复窗口
        _ = User32.PostMessage(new WindowInteropHelper(window).Handle, (int)User32.WindowMessage.WM_NCLBUTTONDOWN, (int)User32.HitTestValues.HTCAPTION, 0);
        # 将窗口样式设置为单边框窗口
        window.WindowStyle = WindowStyle.SingleBorderWindow;
    }

    # 标记方法为安全关键方法
    [SecuritySafeCritical]
    # 定义一个公共静态方法，根据当前窗口状态来最大化或恢复窗口
    public static void MaximizeOrRestoreWindow(Window window)
    {
        # 如果窗口当前是最大化状态
        if (window.WindowState == WindowState.Maximized)
        {
            # 恢复窗口到正常状态
            RestoreWindow(window);
        }
        # 如果窗口当前不是最大化状态
        else
        {
            # 最大化窗口
            MaximizeWindow(window);
        }
    }
}
```