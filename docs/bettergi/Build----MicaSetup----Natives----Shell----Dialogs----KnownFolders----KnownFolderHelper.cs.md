# `.\better-genshin-impact\Build\MicaSetup\Natives\Shell\Dialogs\KnownFolders\KnownFolderHelper.cs`

```cs
﻿using System;
using System.Diagnostics;

namespace MicaSetup.Shell.Dialogs;

public static class KnownFolderHelper
{
    // 根据规范名称获取 IKnownFolder 对象
    public static IKnownFolder FromCanonicalName(string canonicalName)
    {
        // 创建 IKnownFolderManager 实例
        var knownFolderManager = (IKnownFolderManager)new KnownFolderManagerClass();

        // 通过规范名称获取原生 IKnownFolder 对象
        knownFolderManager.GetFolderByName(canonicalName, out var knownFolderNative);
        // 获取 IKnownFolder 对象，若失败则抛出异常
        var kf = KnownFolderHelper.GetKnownFolder(knownFolderNative);
        return kf ?? throw new ArgumentException(LocalizedMessages.ShellInvalidCanonicalName, "canonicalName");
    }

    // 根据已知文件夹 ID 获取 IKnownFolder 对象
    public static IKnownFolder FromKnownFolderId(Guid knownFolderId)
    {
        // 创建 KnownFolderManagerClass 实例
        var knownFolderManager = new KnownFolderManagerClass();

        // 获取原生 IKnownFolder 对象，并检查返回结果
        var hr = knownFolderManager.GetFolder(knownFolderId, out var knownFolderNative);
        if (hr != HResult.Ok) { throw new ShellException(hr); }

        // 获取 IKnownFolder 对象，若失败则抛出异常
        var kf = GetKnownFolder(knownFolderNative);
        return kf ?? throw new ArgumentException(LocalizedMessages.KnownFolderInvalidGuid, "knownFolderId");
    }

    // 根据解析名称获取 IKnownFolder 对象
    public static IKnownFolder FromParsingName(string parsingName)
    {
        // 如果解析名称为空，则抛出异常
        if (parsingName == null)
        {
            throw new ArgumentNullException("parsingName");
        }

        nint pidl = 0;
        nint pidl2 = 0;

        try
        {
            // 从解析名称获取 PIDL
            pidl = ShellHelper.PidlFromParsingName(parsingName);

            // 如果 PIDL 无效，则抛出异常
            if (pidl == 0)
            {
                throw new ArgumentException(LocalizedMessages.KnownFolderParsingName, "parsingName");
            }

            // 根据 PIDL 获取原生 IKnownFolder 对象
            var knownFolderNative = KnownFolderHelper.FromPIDL(pidl);
            if (knownFolderNative != null)
            {
                // 获取 IKnownFolder 对象，若失败则抛出异常
                var kf = KnownFolderHelper.GetKnownFolder(knownFolderNative);
                return kf ?? throw new ArgumentException(LocalizedMessages.KnownFolderParsingName, "parsingName");
            }

            // 处理解析名称可能的空字符扩展，获取 PIDL
            pidl2 = ShellHelper.PidlFromParsingName(parsingName.PadRight(1, '\0'));

            // 如果扩展 PIDL 无效，则抛出异常
            if (pidl2 == 0)
            {
                throw new ArgumentException(LocalizedMessages.KnownFolderParsingName, "parsingName");
            }

            // 根据 PIDL 获取 IKnownFolder 对象，若失败则抛出异常
            var kf2 = KnownFolderHelper.GetKnownFolder(KnownFolderHelper.FromPIDL(pidl));
            return kf2 ?? throw new ArgumentException(LocalizedMessages.KnownFolderParsingName, "parsingName");
        }
        finally
        {
            // 释放 PIDL 资源
            Shell32.ILFree(pidl);
            Shell32.ILFree(pidl2);
        }
    }

    // 根据路径获取 IKnownFolder 对象，实际上调用的是 FromParsingName 方法
    public static IKnownFolder FromPath(string path) => KnownFolderHelper.FromParsingName(path);

    // 内部方法，根据已知文件夹 ID 获取 IKnownFolder 对象
    internal static IKnownFolder FromKnownFolderIdInternal(Guid knownFolderId)
    {
        // 创建 IKnownFolderManager 实例
        var knownFolderManager = (IKnownFolderManager)new KnownFolderManagerClass();

        // 获取原生 IKnownFolder 对象，并检查返回结果
        var hr = knownFolderManager.GetFolder(knownFolderId, out var knownFolderNative);

        // 返回 IKnownFolder 对象，若失败返回 null
        return (hr == HResult.Ok) ? GetKnownFolder(knownFolderNative) : null!;
    }

    // 内部方法，根据 PIDL 获取 IKnownFolderNative 对象
    internal static IKnownFolderNative FromPIDL(nint pidl)
    # 创建 KnownFolderManager 类的实例
    {
        var knownFolderManager = new KnownFolder
# 结束当前代码块或语句
}
```