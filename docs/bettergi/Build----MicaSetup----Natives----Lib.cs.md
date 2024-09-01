# `.\better-genshin-impact\Build\MicaSetup\Natives\Lib.cs`

```cs
# 定义一个命名空间，包含各种 DLL 文件的常量
﻿namespace MicaSetup.Natives;

# 定义一个公共静态类，用于存储 DLL 文件名常量
public static class Lib
{
    # 定义一组常量，表示不同的 DLL 文件
    public const string
        User32 = "user32.dll",  # 用户界面相关的 DLL 文件
        Gdi32 = "gdi32.dll",    # 图形设备接口相关的 DLL 文件
        GdiPlus = "gdiplus.dll", # 增强型图形设备接口相关的 DLL 文件
        Kernel32 = "kernel32.dll", # 核心系统功能相关的 DLL 文件
        Shell32 = "shell32.dll", # Windows Shell 相关的 DLL 文件
        SHCore = "shcore.dll",   # Shell 核心功能相关的 DLL 文件
        ShlwApi = "shlwapi.dll", # Shell 轻量级 API 相关的 DLL 文件
        MsImg = "msimg32.dll",   # Microsoft 图像库相关的 DLL 文件
        NTdll = "ntdll.dll",     # NT 内部功能相关的 DLL 文件
        WinInet = "wininet.dll", # Windows Internet 相关的 DLL 文件
        UxTheme = "uxtheme.dll", # 用户体验主题相关的 DLL 文件
        DnsApi = "dnsapi.dll",   # DNS API 相关的 DLL 文件
        DwmApi = "dwmapi.dll",   # 桌面窗口管理器 API 相关的 DLL 文件
        PropSys = "propsys.dll", # 属性系统相关的 DLL 文件
        OleAut32 = "oleaut32.dll", # OLE 自动化相关的 DLL 文件
        Ole32 = "ole32.dll";     # OLE 相关的 DLL 文件
}
```