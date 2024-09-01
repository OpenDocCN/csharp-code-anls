# `.\better-genshin-impact\Build\MicaSetup\Design\Controls\Window\WindowBackdrop.cs`

```cs
﻿using MicaSetup.Helper; // 引入 MicaSetup.Helper 命名空间中的类型和方法
using MicaSetup.Natives; // 引入 MicaSetup.Natives 命名空间中的类型和方法
using System; // 引入 System 命名空间中的基本类型和方法
using System.Runtime.InteropServices; // 引入用于与非托管代码交互的类型
using System.Windows; // 引入 WPF 的核心类型
using System.Windows.Interop; // 引入 WPF 窗口和非托管代码交互的类型
using System.Windows.Media; // 引入 WPF 绘图和媒体功能

namespace MicaSetup.Design.Controls // 定义 MicaSetup.Design.Controls 命名空间
{
    public static class WindowBackdrop // 定义静态类 WindowBackdrop
    {
        private static bool IsSupported(WindowBackdropType backdropType) // 定义检查指定 backdropType 是否被支持的方法
        {
            return backdropType switch // 根据 backdropType 的不同值选择相应的支持情况
            {
                WindowBackdropType.Auto => OsVersionHelper.IsWindows11_22523, // 如果 backdropType 是 Auto，则检查是否是 Windows 11 22523 版本
                WindowBackdropType.Tabbed => OsVersionHelper.IsWindows11_22523, // 如果 backdropType 是 Tabbed，则检查是否是 Windows 11 22523 版本
                WindowBackdropType.Mica => OsVersionHelper.IsWindows11_OrGreater, // 如果 backdropType 是 Mica，则检查是否是 Windows 11 或更高版本
                WindowBackdropType.Acrylic => OsVersionHelper.IsWindows7_OrGreater, // 如果 backdropType 是 Acrylic，则检查是否是 Windows 7 或更高版本
                WindowBackdropType.None => OsVersionHelper.IsWindows11_OrGreater, // 如果 backdropType 是 None，则检查是否是 Windows 11 或更高版本
                _ => false // 对于其他未定义的 backdropType 返回 false
            };
        }

        public static bool ApplyBackdrop(Window window, WindowBackdropType backdropType = WindowBackdropType.Mica, ApplicationTheme theme = ApplicationTheme.Unknown) // 定义应用指定 backdropType 的背景的方法，默认使用 Mica 背景和未知主题
        {
            if (!IsSupported(backdropType)) // 检查指定的 backdropType 是否被支持
            {
                return false; // 如果不支持，返回 false
            }

            if (window is null) // 检查传入的 window 对象是否为 null
            {
                return false; // 如果是 null，返回 false
            }

            if (window.IsLoaded) // 检查 window 是否已经加载
            {
                nint windowHandle = new WindowInteropHelper(window).Handle; // 获取 window 的窗口句柄

                if (windowHandle == 0x00) // 检查句柄是否为无效值
                {
                    return false; // 如果无效，返回 false
                }

                return ApplyBackdrop(windowHandle, backdropType, theme); // 调用重载的 ApplyBackdrop 方法应用背景
            }

            window.Loaded += (sender, _) => // 为 window 的 Loaded 事件添加处理程序
            {
                nint windowHandle =
                    new WindowInteropHelper(sender as Window ?? null)?.Handle // 获取事件源的窗口句柄
                    ?? IntPtr.Zero; // 如果事件源为 null，则使用 IntPtr.Zero 作为默认值

                if (windowHandle == 0x00) // 检查句柄是否为无效值
                    return; // 如果无效，直接返回

                ApplyBackdrop(windowHandle, backdropType, theme); // 调用重载的 ApplyBackdrop 方法应用背景
            };

            return true; // 返回 true 表示已成功设置事件处理程序
        }

        public static bool ApplyBackdrop(nint hWnd, WindowBackdropType backdropType = WindowBackdropType.Mica, ApplicationTheme theme = ApplicationTheme.Unknown) // 定义应用指定 backdropType 的背景的方法，使用窗口句柄
    # 检查给定的 backdropType 是否被支持，不支持则返回 false
    {
        if (!IsSupported(backdropType))
        {
            return false;
        }

        # 检查窗口句柄是否为零或窗口是否有效，不有效则返回 false
        if (hWnd == 0x00 || !User32.IsWindow(hWnd))
        {
            return false;
        }

        # 如果 backdropType 不是自动类型，则移除窗口背景
        if (backdropType != WindowBackdropType.Auto)
        {
            WindowDarkMode.RemoveBackground(hWnd);
        }

        # 根据指定的主题设置窗口暗模式或移除暗模式
        switch (theme)
        {
            case ApplicationTheme.Unknown:
                if (ApplicationThemeManager.GetAppTheme() == ApplicationTheme.Dark)
                {
                    WindowDarkMode.ApplyWindowDarkMode(hWnd);
                }
                break;

            case ApplicationTheme.Dark:
                WindowDarkMode.ApplyWindowDarkMode(hWnd);
                break;

            case ApplicationTheme.Light:
            case ApplicationTheme.HighContrast:
                WindowDarkMode.RemoveWindowDarkMode(hWnd);
                break;
        }

        # 设置窗口主题属性，防止绘制标题栏
        var wtaOptions = new UxTheme.WTA_OPTIONS()
        {
            Flags = UxTheme.WTNCA.WTNCA_NODRAWCAPTION,
            Mask = (uint)UxTheme.ThemeDialogTextureFlags.ETDT_VALIDBITS,
        };

        UxTheme.SetWindowThemeAttribute(
            hWnd,
            UxTheme.WINDOWTHEMEATTRIBUTETYPE.WTA_NONCLIENT,
            wtaOptions,
            (uint)Marshal.SizeOf(typeof(UxTheme.WTA_OPTIONS))
        );

        # 设置窗口系统背景类型，决定背景风格
        var dwmApiResult = DwmApi.DwmSetWindowAttribute(
            hWnd,
            DWMWINDOWATTRIBUTE.DWMWA_SYSTEMBACKDROP_TYPE,
            (int)(backdropType switch
            {
                WindowBackdropType.Auto => DwmApi.DWM_SYSTEMBACKDROP_TYPE.DWMSBT_AUTO,
                WindowBackdropType.Mica => DwmApi.DWM_SYSTEMBACKDROP_TYPE.DWMSBT_MAINWINDOW,
                WindowBackdropType.Acrylic => DwmApi.DWM_SYSTEMBACKDROP_TYPE.DWMSBT_TRANSIENTWINDOW,
                WindowBackdropType.Tabbed => DwmApi.DWM_SYSTEMBACKDROP_TYPE.DWMSBT_TABBEDWINDOW,
                _ => DwmApi.DWM_SYSTEMBACKDROP_TYPE.DWMSBT_NONE,
            }),
            Marshal.SizeOf(typeof(int))
        );

        # 返回 DWM API 调用结果是否成功
        return dwmApiResult == HRESULT.S_OK;
    }

    # 从 Window 对象中移除背景，如果 window 为 null 则返回 false
    public static bool RemoveBackdrop(Window window)
    {
        if (window == null)
        {
            return false;
        }

        # 获取窗口句柄
        nint hWnd = new WindowInteropHelper(window).Handle;

        # 调用另一个重载的 RemoveBackdrop 方法处理实际移除背景的逻辑
        return RemoveBackdrop(hWnd);
    }

    # 移除指定窗口的背景
    public static bool RemoveBackdrop(nint hWnd)
    # 检查窗口句柄是否为0或窗口句柄无效
    if (hWnd == 0x00 || !User32.IsWindow(hWnd))
    {
        # 如果无效，返回 false
        return false;
    }

    # 从窗口句柄创建 HwndSource 对象
    var windowSource = HwndSource.FromHwnd(hWnd);

    # 检查 windowSource 是否有效且其 CompositionTarget 不为 null
    if (windowSource?.Handle.ToInt32() != 0x00 && windowSource?.CompositionTarget != null)
    {
        # 设置窗口的背景颜色为系统窗口颜色
        windowSource.CompositionTarget.BackgroundColor = SystemColors.WindowColor;
    }

    # 检查 windowSource 的 RootVisual 是否为 Window 类型
    if (windowSource?.RootVisual is Window window)
    {
        # 从窗口资源中获取名为 "ApplicationBackgroundBrush" 的背景画刷
        var backgroundBrush = window.Resources["ApplicationBackgroundBrush"];

        # 如果获取到的背景画刷不是 SolidColorBrush 类型
        if (backgroundBrush is not SolidColorBrush)
        {
            # 根据应用程序主题设置背景画刷的颜色
            backgroundBrush = ApplicationThemeManager.GetAppTheme() == ApplicationTheme.Dark
                                ? new SolidColorBrush(Color.FromArgb(0xFF, 0x20, 0x20, 0x20))  # 黑暗主题背景色
                                : new SolidColorBrush(Color.FromArgb(0xFF, 0xFA, 0xFA, 0xFA));  # 浅色主题背景色
        }

        # 设置窗口的背景为指定的背景画刷
        window.Background = (SolidColorBrush)backgroundBrush;
    }

    # 调用 DwmSetWindowAttribute 函数，设置窗口的 Mica 效果属性
    _ = DwmApi.DwmSetWindowAttribute(
        hWnd,
        DWMWINDOWATTRIBUTE.DWMWA_MICA_EFFECT,  # 属性标识符
        0x0,  # 属性值
        Marshal.SizeOf(typeof(int))  # 属性值的大小
    );

    # 调用 DwmSetWindowAttribute 函数，设置窗口的系统背景类型属性
    _ = DwmApi.DwmSetWindowAttribute(
        hWnd,
        DWMWINDOWATTRIBUTE.DWMWA_SYSTEMBACKDROP_TYPE,  # 属性标识符
        (int)DwmApi.DWM_SYSTEMBACKDROP_TYPE.DWMSBT_NONE,  # 属性值
        Marshal.SizeOf(typeof(int))  # 属性值的大小
    );

    # 返回 true，表示操作成功
    return true;
# 代码块的结束符号
}
```