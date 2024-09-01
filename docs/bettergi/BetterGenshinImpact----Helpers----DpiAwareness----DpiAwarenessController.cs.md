# `.\better-genshin-impact\BetterGenshinImpact\Helpers\DpiAwareness\DpiAwarenessController.cs`

```cs
﻿using System;
using System.Diagnostics;
using System.Windows;
using System.Windows.Interop;
using System.Windows.Media;
using Vanara.PInvoke;

namespace BetterGenshinImpact.Helpers.DpiAwareness;

/// <summary>
/// 高分辨率适配器
/// </summary>
internal class DpiAwarenessController
{
    private readonly Window window;  // 存储目标窗体实例

    private HwndSource? hwndSource;  // 用于存储窗口源的可选对象
    private HWND? hwnd;  // 存储窗口句柄的可选对象
    private double currentDpiRatio;  // 当前 DPI 比例

    static DpiAwarenessController()
    {
        // 设置进程 DPI 感知模式为每个监视器独立 DPI 感知
        SHCore.SetProcessDpiAwareness(SHCore.PROCESS_DPI_AWARENESS.PROCESS_PER_MONITOR_DPI_AWARE).ThrowIfFailed();
    }

    /// <summary>
    /// 构造一个新的高分辨率适配器
    /// </summary>
    /// <param name="window">目标窗体</param>
    public DpiAwarenessController(Window window)
    {
        this.window = window;  // 初始化目标窗体
        window.Loaded += (_, _) => OnAttached();  // 窗体加载时调用 OnAttached 方法
        window.Closing += (_, _) => OnDetaching();  // 窗体关闭时调用 OnDetaching 方法
    }

    private void OnAttached()
    {
        if (window.IsInitialized)  // 如果窗体已经初始化
        {
            AddHwndHook();  // 添加窗口钩子
        }
        else
        {
            window.SourceInitialized += AssociatedWindowSourceInitialized;  // 窗体源初始化时调用 AssociatedWindowSourceInitialized 方法
        }
    }

    private void OnDetaching()
    {
        RemoveHwndHook();  // 移除窗口钩子
    }

    private void AddHwndHook()
    {
        hwndSource = PresentationSource.FromVisual(window) as HwndSource;  // 从窗体获取窗口源
        hwndSource?.AddHook(HwndHook);  // 如果获取成功，则添加钩子
        hwnd = new WindowInteropHelper(window).Handle;  // 获取窗体句柄
    }

    private void RemoveHwndHook()
    {
        window.SourceInitialized -= AssociatedWindowSourceInitialized;  // 移除窗体源初始化事件处理程序
        hwndSource?.RemoveHook(HwndHook);  // 如果窗口源存在，则移除钩子
        hwnd = null;  // 置空窗口句柄
    }

    private void AssociatedWindowSourceInitialized(object? sender, EventArgs e)
    {
        AddHwndHook();  // 添加窗口钩子

        currentDpiRatio = GetScaleRatio(window);  // 获取并存储当前 DPI 比例
        UpdateDpiScaling(currentDpiRatio, true);  // 更新 DPI 缩放
    }

    private unsafe nint HwndHook(nint hWnd, int message, nint wParam, nint lParam, ref bool handled)
    {
        if (message is 0x02E0)  // 如果消息是 WM_DPICHANGED
        {
            RECT rect = *(RECT*)&lParam;  // 从 lParam 获取窗口的 RECT 信息

            User32.SetWindowPosFlags flag =
                User32.SetWindowPosFlags.SWP_NOZORDER
                | User32.SetWindowPosFlags.SWP_NOACTIVATE
                | User32.SetWindowPosFlags.SWP_NOOWNERZORDER;  // 设置窗口位置标志
            User32.SetWindowPos(hWnd, default, rect.Left, rect.Top, rect.Right - rect.Left, rect.Bottom - rect.Top, flag);  // 调整窗口位置和大小

            // we modified this fragment to correct the wrong behaviour
            double newDpiRatio = GetScaleRatio(window) * currentDpiRatio;  // 计算新的 DPI 比例
            if (newDpiRatio != currentDpiRatio)  // 如果新的 DPI 比例不同于当前比例
            {
                UpdateDpiScaling(newDpiRatio);  // 更新 DPI 缩放
            }
        }

        return default;  // 默认返回值
    }

    private void UpdateDpiScaling(double newDpiRatio, bool useSacleCenter = false)  // 更新 DPI 缩放
    # 设置当前的 DPI 比例
    {
        currentDpiRatio = newDpiRatio;
        # 输出当前 DPI 缩放比例到调试窗口，格式化为百分比
        Debug.WriteLine($"Set dpi scaling to {currentDpiRatio:p2}");
        # 获取窗口的第一个子元素，并将其转换为 FrameworkElement
        FrameworkElement firstChild = (FrameworkElement)VisualTreeHelper.GetChild(window, 0);
        ScaleTransform transform;
        # 如果使用缩放中心
        if (useSacleCenter)
        {
            # 计算窗口中心的 X 坐标
            double centerX = window.Left + (window.Width / 2);
            # 计算窗口中心的 Y 坐标
            double centerY = window.Top + (window.Height / 2);

            # 创建一个 ScaleTransform 对象，使用中心点进行缩放
            transform = new ScaleTransform(currentDpiRatio, currentDpiRatio, centerX, centerY);
        }
        else
        {
            # 创建一个 ScaleTransform 对象，不使用中心点进行缩放
            transform = new ScaleTransform(currentDpiRatio, currentDpiRatio);
        }

        # 将创建的变换应用到第一个子元素的布局变换属性上
        firstChild.LayoutTransform = transform;
    }
    # 获取窗口的缩放比例
    private static double GetScaleRatio(Window window)
    {
        # 获取窗口的 PresentationSource 对象
        PresentationSource hwndSource = PresentationSource.FromVisual(window);

        # TODO: 验证是否使用 hwndSource
        # 计算 WPF 的 DPI 值
        double wpfDpi = 96.0 * hwndSource.CompositionTarget.TransformToDevice.M11;

        # 获取监视器句柄
        HMONITOR hMonitor = User32.MonitorFromWindow(((HwndSource)hwndSource).Handle, User32.MonitorFlags.MONITOR_DEFAULTTONEAREST);
        # 获取监视器的 DPI 值
        _ = SHCore.GetDpiForMonitor(hMonitor, SHCore.MONITOR_DPI_TYPE.MDT_EFFECTIVE_DPI, out uint dpiX, out uint _);
        # 返回 DPI 比例
        return dpiX / wpfDpi;
    }
请提供更详细的代码示例，以便我能正确添加注释。
```