# `.\better-genshin-impact\Build\MicaSetup\Natives\DwmApi.cs`

```cs
# 引入用于平台调用的命名空间
﻿using System.Runtime.InteropServices;

# 定义命名空间 MicaSetup.Natives
namespace MicaSetup.Natives;

# 定义一个静态类 DwmApi
public static class DwmApi
{
    # 定义一个外部方法 DwmSetWindowAttribute，用于调用 Windows API
    [DllImport(Lib.DwmApi)]
    [DefaultDllImportSearchPaths(DllImportSearchPath.System32)]
    public static extern int DwmSetWindowAttribute(nint hwnd, DWMWINDOWATTRIBUTE dwAttribute, ref int pvAttribute, int cbAttribute);

    # 定义一个重载方法 DwmSetWindowAttribute，简化 API 调用
    public static int DwmSetWindowAttribute(nint hwnd, DWMWINDOWATTRIBUTE dwAttribute, int pvAttribute, int cbAttribute)
    {
        # 调用外部方法 DwmSetWindowAttribute，并传递 pvAttribute 的引用
        return DwmSetWindowAttribute(hwnd, dwAttribute, ref pvAttribute, cbAttribute);
    }

    # 定义枚举 DWM_SYSTEMBACKDROP_TYPE，用于指定系统背景效果类型
    public enum DWM_SYSTEMBACKDROP_TYPE
    {
        # 默认值，让桌面窗口管理器（DWM）自动决定窗口的系统背景材质
        DWMSBT_AUTO,

        # 不绘制任何系统背景
        DWMSBT_NONE,

        # 绘制对应于长期存在窗口的背景材质效果
        DWMSBT_MAINWINDOW,

        # 绘制对应于临时窗口的背景材质效果
        DWMSBT_TRANSIENTWINDOW,

        # 绘制对应于有标签标题栏的窗口的背景材质效果
        DWMSBT_TABBEDWINDOW,
    }
}
```