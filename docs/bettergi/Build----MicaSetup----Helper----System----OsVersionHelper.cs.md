# `.\better-genshin-impact\Build\MicaSetup\Helper\System\OsVersionHelper.cs`

```cs
﻿using MicaSetup.Natives;
using System;

namespace MicaSetup.Helper;

public static class OsVersionHelper
{
    // 存储操作系统版本的缓存
    private static Version? versionCache;
    // 获取操作系统版本，并将其存储在只读字段中
    private static readonly Version _osVersion = GetOSVersion();

    // 判断操作系统是否为 Windows NT
    public static bool IsWindowsNT { get; } = Environment.OSVersion.Platform == PlatformID.Win32NT;
    // 判断操作系统是否为 Windows XP
    public static bool IsWindowsXP { get; } = IsWindowsNT && _osVersion == new Version(5, 0);
    // 判断操作系统是否为 Windows XP 或更高版本
    public static bool IsWindowsXP_OrGreater { get; } = IsWindowsNT && _osVersion >= new Version(5, 0);
    // 判断操作系统是否为 Windows Vista
    public static bool IsWindowsVista { get; } = IsWindowsNT && _osVersion == new Version(6, 0);
    // 判断操作系统是否为 Windows Vista 或更高版本
    public static bool IsWindowsVista_OrGreater { get; } = IsWindowsNT && _osVersion >= new Version(6, 0);
    // 判断操作系统是否为 Windows 7
    public static bool IsWindows7 { get; } = IsWindowsNT && _osVersion == new Version(6, 1);
    // 判断操作系统是否为 Windows 7 或更高版本
    public static bool IsWindows7_OrGreater { get; } = IsWindowsNT && _osVersion >= new Version(6, 1);
    // 判断操作系统是否为 Windows 8
    public static bool IsWindows8 { get; } = IsWindowsNT && _osVersion == new Version(6, 2);
    // 判断操作系统是否为 Windows 8 或更高版本
    public static bool IsWindows8_OrGreater { get; } = IsWindowsNT && _osVersion >= new Version(6, 2);
    // 判断操作系统是否为 Windows 8.1
    public static bool IsWindows81 { get; } = IsWindowsNT && _osVersion == new Version(6, 3);
    // 判断操作系统是否为 Windows 8.1 或更高版本
    public static bool IsWindows81_OrGreater { get; } = IsWindowsNT && _osVersion >= new Version(6, 3);
    // 判断操作系统是否为 Windows 10
    public static bool IsWindows10 { get; } = IsWindowsNT && _osVersion == new Version(10, 0);
    // 判断操作系统是否为 Windows 10 或更高版本
    public static bool IsWindows10_OrGreater { get; } = IsWindowsNT && _osVersion >= new Version(10, 0);
    // 判断操作系统是否为 Windows 10 1507 版本（build 10240）
    public static bool IsWindows10_1507 { get; } = IsWindowsNT && _osVersion == new Version(10, 0, 10240);
    // 判断操作系统是否为 Windows 10 1507 或更高版本
    public static bool IsWindows10_1507_OrGreater { get; } = IsWindowsNT && _osVersion >= new Version(10, 0, 10240);
    // 判断操作系统是否为 Windows 10 1511 版本（build 10586）
    public static bool IsWindows10_1511 { get; } = IsWindowsNT && _osVersion == new Version(10, 0, 10586);
    // 判断操作系统是否为 Windows 10 1511 或更高版本
    public static bool IsWindows10_1511_OrGreater { get; } = IsWindowsNT && _osVersion >= new Version(10, 0, 10586);
    // 判断操作系统是否为 Windows 10 1607 版本（build 14393）
    public static bool IsWindows10_1607 { get; } = IsWindowsNT && _osVersion == new Version(10, 0, 14393);
    // 判断操作系统是否为 Windows 10 1607 或更高版本
    public static bool IsWindows10_1607_OrGreater { get; } = IsWindowsNT && _osVersion >= new Version(10, 0, 14393);
    // 判断操作系统是否为 Windows 10 1703 版本（build 15063）
    public static bool IsWindows10_1703 { get; } = IsWindowsNT && _osVersion == new Version(10, 0, 15063);
    // 判断操作系统是否为 Windows 10 1703 或更高版本
    public static bool IsWindows10_1703_OrGreater { get; } = IsWindowsNT && _osVersion >= new Version(10, 0, 15063);
    // 判断操作系统是否为 Windows 10 1709 版本（build 16299）
    public static bool IsWindows10_1709 { get; } = IsWindowsNT && _osVersion == new Version(10, 0, 16299);
    // 判断操作系统是否为 Windows 10 1709 或更高版本
    public static bool IsWindows10_1709_OrGreater { get; } = IsWindowsNT && _osVersion >= new Version(10, 0, 16299);
    // 判断操作系统是否为 Windows 10 1803 版本（build 17134）
    public static bool IsWindows10_1803 { get; } = IsWindowsNT && _osVersion == new Version(10, 0, 17134);
    // 判断操作系统是否为 Windows 10 1803 或更高版本
    public static bool IsWindows10_1803_OrGreater { get; } = IsWindowsNT && _osVersion >= new Version(10, 0, 17134);
    // 判断操作系统是否为 Windows 10 1809 版本（build 17763）
    public static bool IsWindows10_1809 { get; } = IsWindowsNT && _osVersion == new Version(10, 0, 17763);
    # 检查是否是 Windows 10 1809 或更高版本
    public static bool IsWindows10_1809_OrGreater { get; } = IsWindowsNT && _osVersion >= new Version(10, 0, 17763);
    # 检查是否是 Windows 10 1903
    public static bool IsWindows10_1903 { get; } = IsWindowsNT && _osVersion == new Version(10, 0, 18362);
    # 检查是否是 Windows 10 1903 或更高版本
    public static bool IsWindows10_1903_OrGreater { get; } = IsWindowsNT && _osVersion >= new Version(10, 0, 18362);
    # 检查是否是 Windows 10 1909
    public static bool IsWindows10_1909 { get; } = IsWindowsNT && _osVersion == new Version(10, 0, 18363);
    # 检查是否是 Windows 10 1909 或更高版本
    public static bool IsWindows10_1909_OrGreater { get; } = IsWindowsNT && _osVersion >= new Version(10, 0, 18363);
    # 检查是否是 Windows 10 2004
    public static bool IsWindows10_2004 { get; } = IsWindowsNT && _osVersion == new Version(10, 0, 19041);
    # 检查是否是 Windows 10 2004 或更高版本
    public static bool IsWindows10_2004_OrGreater { get; } = IsWindowsNT && _osVersion >= new Version(10, 0, 19041);
    # 检查是否是 Windows 10 2009
    public static bool IsWindows10_2009 { get; } = IsWindowsNT && _osVersion == new Version(10, 0, 19042);
    # 检查是否是 Windows 10 2009 或更高版本
    public static bool IsWindows10_2009_OrGreater { get; } = IsWindowsNT && _osVersion >= new Version(10, 0, 19042);
    # 检查是否是 Windows 10 21H1
    public static bool IsWindows10_21H1 { get; } = IsWindowsNT && _osVersion >= new Version(10, 0, 19043);
    # 检查是否是 Windows 10 21H1 或更高版本
    public static bool IsWindows10_21H1_OrGreater { get; } = IsWindowsNT && _osVersion >= new Version(10, 0, 19043);
    # 检查是否是 Windows 11
    public static bool IsWindows11 { get; } = IsWindowsNT && _osVersion == new Version(10, 0, 22000);
    # 检查是否是 Windows 11 或更高版本
    public static bool IsWindows11_OrGreater { get; } = IsWindowsNT && _osVersion >= new Version(10, 0, 22000);
    # 检查是否是 Windows 11 22523
    public static bool IsWindows11_22523 { get; } = IsWindowsNT && _osVersion == new Version(10, 0, 22523);
    # 检查是否是 Windows 11 22523 或更高版本
    public static bool IsWindows11_22523_OrGreater { get; } = IsWindowsNT && _osVersion >= new Version(10, 0, 22523);

    # 获取操作系统版本，如果缓存为空，则从系统中获取并缓存
    public static Version GetOSVersion()
    {
        # 如果版本缓存为空
        if (versionCache is null)
        {
            # 从 NTdll 获取操作系统版本信息，如果获取失败，抛出不支持的平台异常
            if (NTdll.RtlGetVersion(out var osv) != 0)
            {
                throw new PlatformNotSupportedException("Setup can only run on Windows.");
            }

            # 使用操作系统版本信息创建并缓存版本对象
            versionCache = new Version(osv.MajorVersion, osv.MinorVersion, osv.BuildNumber, osv.PlatformId);
        }
        # 返回缓存的操作系统版本
        return versionCache;
    }

    # 如果当前操作系统不是 Vista 或更高版本，抛出不支持的平台异常
    public static void ThrowIfNotVista()
    {
        if (!IsWindowsVista_OrGreater)
        {
            throw new PlatformNotSupportedException("Only supported on Windows Vista or newer.");
        }
    }

    # 如果当前操作系统不是 Windows 7 或更高版本，抛出不支持的平台异常
    public static void ThrowIfNotWin7()
    {
        if (!IsWindows7_OrGreater)
        {
            throw new PlatformNotSupportedException("Only supported on Windows 7 or newer.");
        }
    }

    # 如果当前操作系统不是 Windows XP 或更高版本，抛出不支持的平台异常
    public static void ThrowIfNotXP()
    {
        if (!IsWindowsXP_OrGreater)
        {
            throw new PlatformNotSupportedException("Only supported on Windows XP or newer.");
        }
    }
能否提供更多上下文或代码片段？我需要知道这个代码片段是如何融入整体的。
```