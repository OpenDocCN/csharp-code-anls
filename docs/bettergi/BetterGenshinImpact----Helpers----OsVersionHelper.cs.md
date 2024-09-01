# `.\better-genshin-impact\BetterGenshinImpact\Helpers\OsVersionHelper.cs`

```cs
﻿// 禁用 ReSharper 的不一致命名检查
﻿// ReSharper disable InconsistentNaming
using System;  // 引入 System 命名空间
using Vanara.PInvoke;  // 引入 Vanara.PInvoke 命名空间

namespace BetterGenshinImpact.Helpers;  // 定义命名空间 BetterGenshinImpact.Helpers

public static class OsVersionHelper  // 定义一个静态类 OsVersionHelper
{
    private static Version? versionCache;  // 声明一个可空的静态字段 versionCache，用于缓存操作系统版本
    private static readonly Version _osVersion = GetOSVersion();  // 声明一个静态 readonly 字段 _osVersion，并初始化为操作系统版本

    public static bool IsWindowsNT { get; } = Environment.OSVersion.Platform == PlatformID.Win32NT;  // 判断当前操作系统是否是 Windows NT 系列
    public static bool IsWindowsXP { get; } = IsWindowsNT && _osVersion == new Version(5, 0);  // 判断是否是 Windows XP
    public static bool IsWindowsXP_OrGreater { get; } = IsWindowsNT && _osVersion >= new Version(5, 0);  // 判断是否是 Windows XP 或更高版本
    public static bool IsWindowsVista { get; } = IsWindowsNT && _osVersion == new Version(6, 0);  // 判断是否是 Windows Vista
    public static bool IsWindowsVista_OrGreater { get; } = IsWindowsNT && _osVersion >= new Version(6, 0);  // 判断是否是 Windows Vista 或更高版本
    public static bool IsWindows7 { get; } = IsWindowsNT && _osVersion == new Version(6, 1);  // 判断是否是 Windows 7
    public static bool IsWindows7_OrGreater { get; } = IsWindowsNT && _osVersion >= new Version(6, 1);  // 判断是否是 Windows 7 或更高版本
    public static bool IsWindows8 { get; } = IsWindowsNT && _osVersion == new Version(6, 2);  // 判断是否是 Windows 8
    public static bool IsWindows8_OrGreater { get; } = IsWindowsNT && _osVersion >= new Version(6, 2);  // 判断是否是 Windows 8 或更高版本
    public static bool IsWindows81 { get; } = IsWindowsNT && _osVersion == new Version(6, 3);  // 判断是否是 Windows 8.1
    public static bool IsWindows81_OrGreater { get; } = IsWindowsNT && _osVersion >= new Version(6, 3);  // 判断是否是 Windows 8.1 或更高版本
    public static bool IsWindows10 { get; } = IsWindowsNT && _osVersion == new Version(10, 0);  // 判断是否是 Windows 10
    public static bool IsWindows10_OrGreater { get; } = IsWindowsNT && _osVersion >= new Version(10, 0);  // 判断是否是 Windows 10 或更高版本
    public static bool IsWindows10_1507 { get; } = IsWindowsNT && _osVersion == new Version(10, 0, 10240);  // 判断是否是 Windows 10 版本 1507
    public static bool IsWindows10_1507_OrGreater { get; } = IsWindowsNT && _osVersion >= new Version(10, 0, 10240);  // 判断是否是 Windows 10 版本 1507 或更高版本
    public static bool IsWindows10_1511 { get; } = IsWindowsNT && _osVersion == new Version(10, 0, 10586);  // 判断是否是 Windows 10 版本 1511
    public static bool IsWindows10_1511_OrGreater { get; } = IsWindowsNT && _osVersion >= new Version(10, 0, 10586);  // 判断是否是 Windows 10 版本 1511 或更高版本
    public static bool IsWindows10_1607 { get; } = IsWindowsNT && _osVersion == new Version(10, 0, 14393);  // 判断是否是 Windows 10 版本 1607
    public static bool IsWindows10_1607_OrGreater { get; } = IsWindowsNT && _osVersion >= new Version(10, 0, 14393);  // 判断是否是 Windows 10 版本 1607 或更高版本
    public static bool IsWindows10_1703 { get; } = IsWindowsNT && _osVersion == new Version(10, 0, 15063);  // 判断是否是 Windows 10 版本 1703
    public static bool IsWindows10_1703_OrGreater { get; } = IsWindowsNT && _osVersion >= new Version(10, 0, 15063);  // 判断是否是 Windows 10 版本 1703 或更高版本
    public static bool IsWindows10_1709 { get; } = IsWindowsNT && _osVersion == new Version(10, 0, 16299);  // 判断是否是 Windows 10 版本 1709
    public static bool IsWindows10_1709_OrGreater { get; } = IsWindowsNT && _osVersion >= new Version(10, 0, 16299);  // 判断是否是 Windows 10 版本 1709 或更高版本
    public static bool IsWindows10_1803 { get; } = IsWindowsNT && _osVersion == new Version(10, 0, 17134);  // 判断是否是 Windows 10 版本 1803
    public static bool IsWindows10_1803_OrGreater { get; } = IsWindowsNT && _osVersion >= new Version(10, 0, 17134);  // 判断是否是 Windows 10 版本 1803 或更高版本
    # 判断当前操作系统是否是 Windows 10 版本 1809
    public static bool IsWindows10_1809 { get; } = IsWindowsNT && _osVersion == new Version(10, 0, 17763);
    # 判断当前操作系统是否是 Windows 10 版本 1809 或更高版本
    public static bool IsWindows10_1809_OrGreater { get; } = IsWindowsNT && _osVersion >= new Version(10, 0, 17763);
    # 判断当前操作系统是否是 Windows 10 版本 1903
    public static bool IsWindows10_1903 { get; } = IsWindowsNT && _osVersion == new Version(10, 0, 18362);
    # 判断当前操作系统是否是 Windows 10 版本 1903 或更高版本
    public static bool IsWindows10_1903_OrGreater { get; } = IsWindowsNT && _osVersion >= new Version(10, 0, 18362);
    # 判断当前操作系统是否是 Windows 10 版本 1909
    public static bool IsWindows10_1909 { get; } = IsWindowsNT && _osVersion == new Version(10, 0, 18363);
    # 判断当前操作系统是否是 Windows 10 版本 1909 或更高版本
    public static bool IsWindows10_1909_OrGreater { get; } = IsWindowsNT && _osVersion >= new Version(10, 0, 18363);
    # 判断当前操作系统是否是 Windows 10 版本 2004
    public static bool IsWindows10_2004 { get; } = IsWindowsNT && _osVersion == new Version(10, 0, 19041);
    # 判断当前操作系统是否是 Windows 10 版本 2004 或更高版本
    public static bool IsWindows10_2004_OrGreater { get; } = IsWindowsNT && _osVersion >= new Version(10, 0, 19041);
    # 判断当前操作系统是否是 Windows 10 版本 2009
    public static bool IsWindows10_2009 { get; } = IsWindowsNT && _osVersion == new Version(10, 0, 19042);
    # 判断当前操作系统是否是 Windows 10 版本 2009 或更高版本
    public static bool IsWindows10_2009_OrGreater { get; } = IsWindowsNT && _osVersion >= new Version(10, 0, 19042);
    # 判断当前操作系统是否是 Windows 10 版本 21H1 或更高版本
    public static bool IsWindows10_21H1 { get; } = IsWindowsNT && _osVersion >= new Version(10, 0, 19043);
    # 判断当前操作系统是否是 Windows 10 版本 21H1 或更高版本
    public static bool IsWindows10_21H1_OrGreater { get; } = IsWindowsNT && _osVersion >= new Version(10, 0, 19043);
    # 判断当前操作系统是否是 Windows 11 版本
    public static bool IsWindows11 { get; } = IsWindowsNT && _osVersion == new Version(10, 0, 22000);
    # 判断当前操作系统是否是 Windows 11 或更高版本
    public static bool IsWindows11_OrGreater { get; } = IsWindowsNT && _osVersion >= new Version(10, 0, 22000);
    # 判断当前操作系统是否是 Windows 11 版本 22523
    public static bool IsWindows11_22523 { get; } = IsWindowsNT && _osVersion == new Version(10, 0, 22523);
    # 判断当前操作系统是否是 Windows 11 版本 22523 或更高版本
    public static bool IsWindows11_22523_OrGreater { get; } = IsWindowsNT && _osVersion >= new Version(10, 0, 22523);

    # 获取操作系统版本信息
    public static Version GetOSVersion()
    {
        # 如果版本缓存为空，则获取当前操作系统版本
        if (versionCache is null)
        {
            # 调用系统 API 获取版本信息，若失败则抛出异常
            if (NtDll.RtlGetVersion(out var osv) != 0)
            {
                throw new PlatformNotSupportedException("Setup can only run on Windows.");
            }

            # 将获取到的版本信息封装成 Version 对象并缓存
            versionCache = new Version((int)osv.dwMajorVersion, (int)osv.dwMinorVersion, (int)osv.dwBuildNumber, (int)osv.dwPlatformId);
        }
        # 返回操作系统版本信息
        return versionCache;
    }

    # 如果操作系统版本低于 Windows Vista，则抛出异常
    public static void ThrowIfNotVista()
    {
        if (!IsWindowsVista_OrGreater)
        {
            throw new PlatformNotSupportedException("Only supported on Windows Vista or newer.");
        }
    }

    # 如果操作系统版本低于 Windows 7，则抛出异常
    public static void ThrowIfNotWin7()
    {
        if (!IsWindows7_OrGreater)
        {
            throw new PlatformNotSupportedException("Only supported on Windows 7 or newer.");
        }
    }

    # 如果操作系统版本低于 Windows XP，则抛出异常
    public static void ThrowIfNotXP()
    {
        if (!IsWindowsXP_OrGreater)
        {
            throw new PlatformNotSupportedException("Only supported on Windows XP or newer.");
        }
    }
# 结束了一个代码块或函数定义的范围
}
```