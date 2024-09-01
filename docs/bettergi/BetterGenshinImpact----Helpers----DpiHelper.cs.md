# `.\better-genshin-impact\BetterGenshinImpact\Helpers\DpiHelper.cs`

```cs
﻿using System; // 引入 System 命名空间
using System.Diagnostics; // 引入用于调试的命名空间
using System.Windows; // 引入 WPF 的命名空间
using System.Windows.Interop; // 引入用于窗口互操作的命名空间
using Vanara.PInvoke; // 引入 Vanara PInvoke 库

namespace BetterGenshinImpact.Helpers; // 定义命名空间

public class DpiHelper // 定义 DpiHelper 类
{
    public static float ScaleY => GetScaleY(); // 公开的静态属性，用于获取当前 DPI 缩放的 Y 轴比例

    private static float GetScaleY() // 私有的静态方法，用于获取当前 DPI 缩放的 Y 轴比例
    {
        if (Environment.OSVersion.Version >= new Version(6, 3) // 检查操作系统版本是否为 Windows 8.1 或更高
         && UIDispatcherHelper.MainWindow != null) // 确保当前有主窗口
        {
            HWND hWnd = new WindowInteropHelper(Application.Current?.MainWindow).Handle; // 获取当前主窗口的句柄
            HMONITOR hMonitor = User32.MonitorFromWindow(hWnd, User32.MonitorFlags.MONITOR_DEFAULTTONEAREST); // 获取与窗口关联的显示器句柄
            SHCore.GetDpiForMonitor(hMonitor, SHCore.MONITOR_DPI_TYPE.MDT_EFFECTIVE_DPI, out _, out uint dpiY); // 获取显示器的 DPI 值
            return dpiY / 96f; // 计算并返回 Y 轴的缩放比例
        }

        HDC hdc = User32.GetDC(HWND.NULL); // 获取设备上下文句柄
        float scaleY = Gdi32.GetDeviceCaps(hdc, Gdi32.DeviceCap.LOGPIXELSY); // 获取 Y 轴的 DPI 缩放
        _ = User32.ReleaseDC(0, hdc); // 释放设备上下文句柄
        return scaleY / 96f; // 计算并返回 Y 轴的缩放比例
    }

    public static DpiScaleF GetScale(nint hWnd = 0) // 公开的静态方法，用于获取给定窗口句柄的 DPI 缩放
    {
        if (hWnd != IntPtr.Zero) // 如果提供了有效的窗口句柄
        {
            HMONITOR hMonitor = User32.MonitorFromWindow(hWnd, User32.MonitorFlags.MONITOR_DEFAULTTONEAREST); // 获取与窗口关联的显示器句柄
            SHCore.GetDpiForMonitor(hMonitor, SHCore.MONITOR_DPI_TYPE.MDT_EFFECTIVE_DPI, out uint dpiX, out uint dpiY); // 获取显示器的 DPI 值
            return new DpiScaleF(dpiX / 96f, dpiY / 96f); // 计算并返回 DPI 缩放对象
        }

        using User32.SafeReleaseHDC hdc = User32.GetDC(hWnd); // 获取设备上下文并在使用后释放
        float scaleX = Gdi32.GetDeviceCaps(hdc, Gdi32.DeviceCap.LOGPIXELSX); // 获取 X 轴的 DPI 缩放
        float scaleY = Gdi32.GetDeviceCaps(hdc, Gdi32.DeviceCap.LOGPIXELSY); // 获取 Y 轴的 DPI 缩放
        return new(scaleX / 96f, scaleY / 96f); // 计算并返回 DPI 缩放对象
    }
}

[DebuggerDisplay("{ToString()}")] // 设置调试显示信息
public readonly struct DpiScaleF(float x = 1f, float y = 1f) // 定义只读结构体 DpiScaleF，表示 DPI 缩放
{
    private readonly float x = x; // 只读字段，表示 X 轴的 DPI 缩放
    public float X => x; // 公开属性，获取 X 轴的 DPI 缩放

    private readonly float y = y; // 只读字段，表示 Y 轴的 DPI 缩放
    public float Y => y; // 公开属性，获取 Y 轴的 DPI 缩放

    public DpiScaleF Reserve() // 返回与当前 DPI 缩放相反的缩放对象
    {
        return new DpiScaleF(1f / x, 1f / y); // 创建并返回一个新的 DpiScaleF 对象，其中缩放因子为当前缩放因子的倒数
    }

    public override string ToString() => $"{X},{Y}"; // 重写 ToString 方法，返回格式化的 DPI 缩放字符串
}
```