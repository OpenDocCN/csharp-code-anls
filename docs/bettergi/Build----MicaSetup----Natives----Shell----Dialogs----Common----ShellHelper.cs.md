# `.\better-genshin-impact\Build\MicaSetup\Natives\Shell\Dialogs\Common\ShellHelper.cs`

```cs
# 引入所需的系统库和外部函数
﻿using System;
using System.IO;
using System.Runtime.InteropServices;

namespace MicaSetup.Shell.Dialogs;

# 禁用特定的警告
#pragma warning disable IDE0059

# 定义一个静态类 ShellHelper
internal static class ShellHelper
{
    # 声明一个用于 ItemType 的 PropertyKey
    internal static PropertyKey ItemTypePropertyKey = new PropertyKey(new Guid("28636AA6-953D-11D2-B5D6-00C04FD918D0"), 11);

    # 获取指定路径的绝对路径
    internal static string GetAbsolutePath(string path)
    {
        # 如果路径已经是绝对路径，直接返回
        if (Uri.IsWellFormedUriString(path, UriKind.Absolute))
        {
            return path;
        }
        # 否则将相对路径转换为绝对路径并返回
        return Path.GetFullPath((path));
    }

    # 获取指定 IShellItem2 对象的项类型
    internal static string GetItemType(IShellItem2 shellItem)
    {
        # 如果 IShellItem2 对象不为 null
        if (shellItem != null)
        {
            # 使用 ItemTypePropertyKey 获取项类型字符串
            var hr = shellItem.GetString(ref ItemTypePropertyKey, out var itemType);
            # 如果操作成功，返回项类型字符串
            if (hr == HResult.Ok) { return itemType; }
        }

        # 如果无法获取项类型，返回 null
        return null!;
    }

    # 获取指定 IShellItem 对象的解析名称
    internal static string GetParsingName(IShellItem shellItem)
    {
        # 如果 IShellItem 对象为 null，返回 null
        if (shellItem == null) { return null!; }

        string path = null!;

        # 获取 IShellItem 对象的显示名称，使用桌面绝对解析选项
        var hr = shellItem.GetDisplayName(ShellItemDesignNameOptions.DesktopAbsoluteParsing, out nint pszPath);

        # 如果获取显示名称失败，抛出异常
        if (hr != HResult.Ok && hr != HResult.InvalidArguments)
        {
            throw new ShellException(LocalizedMessages.ShellHelperGetParsingNameFailed, hr);
        }

        # 如果 pszPath 不为 0，转换为字符串并释放内存
        if (pszPath != 0)
        {
            path = Marshal.PtrToStringAuto(pszPath);
            Marshal.FreeCoTaskMem(pszPath);
            pszPath = 0;
        }

        # 返回路径字符串
        return path;
    }

    # 从解析名称创建 PIDL
    internal static nint PidlFromParsingName(string name)
    {
        # 调用 SHParseDisplayName 方法获取 PIDL
        var retCode = Shell32.SHParseDisplayName(
            name, 0, out nint pidl, 0,
            out _);

        # 返回 PIDL，如果操作失败返回 0
        return (CoreErrorHelper.Succeeded(retCode) ? pidl : 0);
    }

    # 从 IShellItem 对象获取 PIDL
    internal static nint PidlFromShellItem(IShellItem nativeShellItem)
    {
        # 获取 IShellItem 对象的 IUnknown 指针
        var unknown = Marshal.GetIUnknownForObject(nativeShellItem);
        # 从 IUnknown 指针获取 PIDL
        return PidlFromUnknown(unknown);
    }

    # 从 IUnknown 指针获取 PIDL
    internal static nint PidlFromUnknown(nint unknown)
    {
        # 调用 SHGetIDListFromObject 方法获取 PIDL
        var retCode = Shell32.SHGetIDListFromObject(unknown, out nint pidl);
        # 返回 PIDL，如果操作失败返回 0
        return (CoreErrorHelper.Succeeded(retCode) ? pidl : 0);
    }
}
```