# `.\better-genshin-impact\Fischless.GameCapture\DwmSharedSurface\Helpers\NativeMethods.cs`

```cs
# 引入系统运行时库的互操作功能
﻿using System.Runtime.InteropServices;
# 引入 Vanara.PInvoke 库，以使用 Windows API 函数
using Vanara.PInvoke;

namespace Fischless.GameCapture.DwmSharedSurface.Helpers;

# 定义一个包含本地方法的类
internal class NativeMethods
{

    # 定义一个委托，用于调用 DwmGetDxSharedSurface 函数
    public delegate bool DwmGetDxSharedSurfaceDelegate(IntPtr hWnd, out IntPtr phSurface, out long pAdapterLuid, out long pFmtWindow, out long pPresentFlags, out long pWin32KUpdateId);

    # 声明一个静态委托字段，用于引用 DwmGetDxSharedSurface 函数
    public static DwmGetDxSharedSurfaceDelegate DwmGetDxSharedSurface;

    # 静态构造函数，用于初始化 DwmGetDxSharedSurface 委托
    static NativeMethods()
    {
        # 获取 user32 模块中的 DwmGetDxSharedSurface 函数的地址
        var ptr = Kernel32.GetProcAddress(Kernel32.GetModuleHandle("user32"), "DwmGetDxSharedSurface");
        # 将函数地址转换为 DwmGetDxSharedSurfaceDelegate 委托
        DwmGetDxSharedSurface = Marshal.GetDelegateForFunctionPointer<DwmGetDxSharedSurfaceDelegate>(ptr);
    }
}
```