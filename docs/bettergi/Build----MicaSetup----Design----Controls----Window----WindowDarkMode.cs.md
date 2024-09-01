# `.\better-genshin-impact\Build\MicaSetup\Design\Controls\Window\WindowDarkMode.cs`

```cs
# 引入必要的命名空间
﻿using MicaSetup.Helper;  # 引入 MicaSetup.Helper 命名空间
using MicaSetup.Natives;  # 引入 MicaSetup.Natives 命名空间
using System.Runtime.InteropServices;  # 引入用于与非托管代码交互的命名空间
using System.Windows;  # 引入用于 Windows 窗体的命名空间
using System.Windows.Interop;  # 引入用于窗体与系统之间交互的命名空间
using System.Windows.Media;  # 引入用于处理颜色和绘图的命名空间

namespace MicaSetup.Design.Controls;  # 定义 MicaSetup.Design.Controls 命名空间

public static class WindowDarkMode  # 定义一个静态类 WindowDarkMode
{
    public static bool ApplyWindowDarkMode(nint hWnd)  # 定义静态方法，应用窗口暗色模式
    {
        if (hWnd == 0x00 || !User32.IsWindow(hWnd))  # 检查窗口句柄是否有效
        {
            return false;  # 如果无效，返回 false
        }

        var dwAttribute = DWMWINDOWATTRIBUTE.DWMWA_USE_IMMERSIVE_DARK_MODE;  # 设置属性为使用沉浸式暗色模式

        if (!OsVersionHelper.IsWindows11_22523_OrGreater)  # 如果操作系统版本低于 Windows 11 22523
        {
            dwAttribute = DWMWINDOWATTRIBUTE.DWMWA_USE_IMMERSIVE_DARK_MODE_OLD;  # 使用旧版暗色模式属性
        }

        _ = DwmApi.DwmSetWindowAttribute(  # 调用 DwmSetWindowAttribute 方法设置窗口属性
            hWnd,  # 窗口句柄
            dwAttribute,  # 属性类型
            0x1,  # 属性值
            Marshal.SizeOf(typeof(int))  # 属性值的大小
        );

        return true;  # 返回 true，表示成功应用暗色模式
    }

    public static bool RemoveWindowDarkMode(nint handle)  # 定义静态方法，移除窗口暗色模式
    {
        if (handle == 0x00 || !User32.IsWindow(handle))  # 检查窗口句柄是否有效
        {
            return false;  # 如果无效，返回 false
        }

        var dwAttribute = DWMWINDOWATTRIBUTE.DWMWA_USE_IMMERSIVE_DARK_MODE;  # 设置属性为使用沉浸式暗色模式

        if (!OsVersionHelper.IsWindows11_22523_OrGreater)  # 如果操作系统版本低于 Windows 11 22523
        {
            dwAttribute = DWMWINDOWATTRIBUTE.DWMWA_USE_IMMERSIVE_DARK_MODE_OLD;  # 使用旧版暗色模式属性
        }

        _ = DwmApi.DwmSetWindowAttribute(  # 调用 DwmSetWindowAttribute 方法设置窗口属性
            handle,  # 窗口句柄
            dwAttribute,  # 属性类型
            0x0,  # 属性值，0 代表移除暗色模式
            Marshal.SizeOf(typeof(int))  # 属性值的大小
        );
        return true;  # 返回 true，表示成功移除暗色模式
    }

    public static bool RemoveBackground(nint hWnd)  # 定义静态方法，移除窗口背景
    {
        if (hWnd == 0x00 || !User32.IsWindow(hWnd))  # 检查窗口句柄是否有效
        {
            return false;  # 如果无效，返回 false
        }

        var windowSource = HwndSource.FromHwnd(hWnd);  # 从窗口句柄获取 HwndSource 对象

        if (windowSource?.RootVisual is Window window)  # 如果根视觉对象是窗口
        {
            return RemoveBackground(window);  # 调用另一个方法来移除背景
        }
        return false;  # 如果不是窗口，返回 false
    }

    public static bool RemoveBackground(Window window)  # 定义静态方法，移除窗口背景
    {
        if (window == null)  # 如果窗口对象为空
        {
            return false;  # 返回 false
        }

        window.Background = Brushes.Transparent;  # 设置窗口背景为透明

        nint windowHandle = new WindowInteropHelper(window).Handle;  # 获取窗口的句柄

        if (windowHandle == 0x00)  # 检查窗口句柄是否有效
        {
            return false;  # 如果无效，返回 false
        }

        var windowSource = HwndSource.FromHwnd(windowHandle);  # 从窗口句柄获取 HwndSource 对象

        if (windowSource?.Handle.ToInt32() != 0x00 && windowSource?.CompositionTarget != null)  # 如果窗口源有效且有合成目标
        {
            windowSource.CompositionTarget.BackgroundColor = Colors.Transparent;  # 设置合成目标背景色为透明
        }

        return true;  # 返回 true，表示成功移除背景
    }
}
```