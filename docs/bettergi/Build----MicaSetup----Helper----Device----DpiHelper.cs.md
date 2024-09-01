# `.\better-genshin-impact\Build\MicaSetup\Helper\Device\DpiHelper.cs`

```cs
﻿using MicaSetup.Natives; // 引入 MicaSetup.Natives 命名空间
using System; // 引入 System 命名空间
using System.Windows.Interop; // 引入 System.Windows.Interop 命名空间
using Application = System.Windows.Application; // 引用 System.Windows.Application 类作为 Application

namespace MicaSetup.Helper; // 定义 MicaSetup.Helper 命名空间

public static class DpiHelper // 定义公共静态类 DpiHelper
{
    public static float ScaleX => GetScale().X; // 获取屏幕的 X 轴 DPI 缩放因子
    public static float ScaleY => GetScale().Y; // 获取屏幕的 Y 轴 DPI 缩放因子

    private static DpiScaleF GetScale() // 定义私有静态方法 GetScale，返回 DpiScaleF 类型
    {
        if (OsVersionHelper.IsWindows81_OrGreater && Application.Current?.MainWindow != null) // 如果操作系统版本 >= Windows 8.1 并且当前应用有主窗口
        {
            nint hwnd = new WindowInteropHelper(Application.Current?.MainWindow).Handle; // 获取主窗口的句柄
            nint hMonitor = User32.MonitorFromWindow(hwnd, MonitorFlags.MONITOR_DEFAULTTONEAREST); // 获取与窗口最接近的监视器句柄
            SHCore.GetDpiForMonitor(hMonitor, MONITOR_DPI_TYPE.MDT_EFFECTIVE_DPI, out uint dpiX, out uint dpiY); // 获取监视器的 DPI
            return new DpiScaleF(dpiX / 96f, dpiY / 96f); // 返回 DPI 比例
        }

        nint hdc = User32.GetDC(0); // 获取设备上下文句柄
        float scaleX = Gdi32.GetDeviceCaps(hdc, DeviceCap.LOGPIXELSX); // 获取 X 轴的 DPI
        float scaleY = Gdi32.GetDeviceCaps(hdc, DeviceCap.LOGPIXELSY); // 获取 Y 轴的 DPI
        User32.ReleaseDC(0, hdc); // 释放设备上下文
        return new(scaleX / 96f, scaleY / 96f); // 返回 DPI 比例
    }

    public static double CalcDPI(double src) => Math.Ceiling(src * ScaleX); // 根据 X 轴 DPI 缩放因子计算并返回 double 类型的 DPI 值

    public static float CalcDPI(float src) => (float)Math.Ceiling(src * ScaleX); // 根据 X 轴 DPI 缩放因子计算并返回 float 类型的 DPI 值

    public static double CalcDPIX(double src) => Math.Ceiling(src * ScaleX); // 根据 X 轴 DPI 缩放因子计算并返回 double 类型的 DPI X 值

    public static float CalcDPIX(float src) => (float)Math.Ceiling(src * ScaleX); // 根据 X 轴 DPI 缩放因子计算并返回 float 类型的 DPI X 值

    public static double CalcDPIY(double src) => Math.Ceiling(src * ScaleY); // 根据 Y 轴 DPI 缩放因子计算并返回 double 类型的 DPI Y 值

    public static float CalcDPIY(float src) => (float)Math.Ceiling(src * ScaleY); // 根据 Y 轴 DPI 缩放因子计算并返回 float 类型的 DPI Y 值

    public static double CalcDPi(double src) => Math.Ceiling(src * (1d / ScaleX)); // 根据 X 轴 DPI 缩放因子的倒数计算并返回 double 类型的 DP 值

    public static float CalcDPi(float src) => (float)Math.Ceiling(src * (1f / ScaleX)); // 根据 X 轴 DPI 缩放因子的倒数计算并返回 float 类型的 DP 值

    public static double CalcDPiX(double src) => Math.Ceiling(src * (1d / ScaleX)); // 根据 X 轴 DPI 缩放因子的倒数计算并返回 double 类型的 DP X 值

    public static float CalcDPiX(float src) => (float)Math.Ceiling(src * (1f / ScaleX)); // 根据 X 轴 DPI 缩放因子的倒数计算并返回 float 类型的 DP X 值

    public static double CalcDPiY(double src) => Math.Ceiling(src * (1d / ScaleY)); // 根据 Y 轴 DPI 缩放因子的倒数计算并返回 double 类型的 DP Y 值

    public static float CalcDPiY(float src) => (float)Math.Ceiling(src * (1f / ScaleY)); // 根据 Y 轴 DPI 缩放因子的倒数计算并返回 float 类型的 DP Y 值
}

internal readonly struct DpiScaleF // 定义内部只读结构体 DpiScaleF
{
    private readonly float x; // 定义只读字段 x，表示 X 轴 DPI 缩放因子
    public float X => x; // 获取 X 轴 DPI 缩放因子

    private readonly float y; // 定义只读字段 y，表示 Y 轴 DPI 缩放因子
    public float Y => y; // 获取 Y 轴 DPI 缩放因子

    public DpiScaleF(float x = 1f, float y = 1f) // 构造函数，初始化 x 和 y 字段，默认值为 1
    {
        this.x = x; // 设置 x 字段的值
        this.y = y; // 设置 y 字段的值
    }

    public DpiScaleF Reserve() // 定义 Reserve 方法，返回缩放因子的倒数
    {
        return new DpiScaleF(1f / x, 1f / y); // 返回一个新的 DpiScaleF 对象，其 x 和 y 为当前值的倒数
    }

    public override string ToString() => $"{X},{Y}"; // 重写 ToString 方法，返回 X 和 Y 的字符串表示
}
```