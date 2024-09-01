# `.\better-genshin-impact\Build\MicaSetup\Natives\Shell\Dialogs\KnownFolders\NonFileSystemKnownFolder.cs`

```cs
﻿using System;
using System.Diagnostics;
using System.Runtime.InteropServices;

namespace MicaSetup.Shell.Dialogs;

#pragma warning disable CS8618

// 定义一个非文件系统已知文件夹类，实现了 IKnownFolder 和 IDisposable 接口
public class NonFileSystemKnownFolder : ShellNonFileSystemFolder, IKnownFolder, IDisposable
{
    // 内部字段，存储已知文件夹的本地实现
    private IKnownFolderNative knownFolderNative;
    // 内部字段，存储已知文件夹的设置
    private KnownFolderSettings knownFolderSettings;

    // 构造函数，接受 IShellItem2 对象，并通过基类构造函数初始化
    internal NonFileSystemKnownFolder(IShellItem2 shellItem) : base(shellItem)
    {
    }

    // 构造函数，接受 IKnownFolderNative 对象，初始化内部字段
    internal NonFileSystemKnownFolder(IKnownFolderNative kf)
    {
        // 断言 kf 不为 null
        Debug.Assert(kf != null);
        // 初始化 knownFolderNative 字段
        knownFolderNative = kf!;

        // 创建 IShellItem2 的 GUID，并从 knownFolderNative 获取 shell 项
        var guid = new Guid(ShellIIDGuid.IShellItem2);
        knownFolderNative!.GetShellItem(0, ref guid, out nativeShellItem);
    }

    // 实现 IKnownFolder 接口的 CanonicalName 属性，返回 KnownFolderSettings 的 CanonicalName
    public string CanonicalName => KnownFolderSettings.CanonicalName;

    // 实现 IKnownFolder 接口的 Category 属性，返回 KnownFolderSettings 的 Category
    public FolderCategory Category => KnownFolderSettings.Category;

    // 实现 IKnownFolder 接口的 DefinitionOptions 属性，返回 KnownFolderSettings 的 DefinitionOptions
    public DefinitionOptions DefinitionOptions => KnownFolderSettings.DefinitionOptions;

    // 实现 IKnownFolder 接口的 Description 属性，返回 KnownFolderSettings 的 Description
    public string Description => KnownFolderSettings.Description;

    // 实现 IKnownFolder 接口的 FileAttributes 属性，返回 KnownFolderSettings 的 FileAttributes
    public System.IO.FileAttributes FileAttributes => KnownFolderSettings.FileAttributes;

    // 实现 IKnownFolder 接口的 FolderId 属性，返回 KnownFolderSettings 的 FolderId
    public Guid FolderId => KnownFolderSettings.FolderId;

    // 实现 IKnownFolder 接口的 FolderType 属性，返回 KnownFolderSettings 的 FolderType
    public string FolderType => KnownFolderSettings.FolderType;

    // 实现 IKnownFolder 接口的 FolderTypeId 属性，返回 KnownFolderSettings 的 FolderTypeId
    public Guid FolderTypeId => KnownFolderSettings.FolderTypeId;

    // 实现 IKnownFolder 接口的 LocalizedName 属性，返回 KnownFolderSettings 的 LocalizedName
    public string LocalizedName => KnownFolderSettings.LocalizedName;

    // 实现 IKnownFolder 接口的 LocalizedNameResourceId 属性，返回 KnownFolderSettings 的 LocalizedNameResourceId
    public string LocalizedNameResourceId => KnownFolderSettings.LocalizedNameResourceId;

    // 实现 IKnownFolder 接口的 ParentId 属性，返回 KnownFolderSettings 的 ParentId
    public Guid ParentId => KnownFolderSettings.ParentId;

    // 重写基类的 ParsingName 属性，直接调用基类的实现
    public override string ParsingName => base.ParsingName;

    // 实现 IKnownFolder 接口的 Path 属性，返回 KnownFolderSettings 的 Path
    public string Path => KnownFolderSettings.Path;

    // 实现 IKnownFolder 接口的 PathExists 属性，返回 KnownFolderSettings 的 PathExists
    public bool PathExists => KnownFolderSettings.PathExists;

    // 实现 IKnownFolder 接口的 Redirection 属性，返回 KnownFolderSettings 的 Redirection
    public RedirectionCapability Redirection => KnownFolderSettings.Redirection;

    // 实现 IKnownFolder 接口的 RelativePath 属性，返回 KnownFolderSettings 的 RelativePath
    public string RelativePath => KnownFolderSettings.RelativePath;

    // 实现 IKnownFolder 接口的 Security 属性，返回 KnownFolderSettings 的 Security
    public string Security => KnownFolderSettings.Security;

    // 实现 IKnownFolder 接口的 Tooltip 属性，返回 KnownFolderSettings 的 Tooltip
    public string Tooltip => KnownFolderSettings.Tooltip;

    // 实现 IKnownFolder 接口的 TooltipResourceId 属性，返回 KnownFolderSettings 的 TooltipResourceId
    public string TooltipResourceId => KnownFolderSettings.TooltipResourceId;

    // 属性，获取 KnownFolderSettings 对象，延迟初始化
    private KnownFolderSettings KnownFolderSettings
    {
        get
        {
            // 如果 knownFolderNative 为空
            if (knownFolderNative == null)
            {
                // 如果 nativeShellItem 不为空且 base.PIDL 为 0
                if (nativeShellItem != null && base.PIDL == 0)
                {
                    // 从 nativeShellItem 获取 PIDL
                    base.PIDL = ShellHelper.PidlFromShellItem(nativeShellItem);
                }

                // 如果 base.PIDL 不为 0
                if (base.PIDL != 0)
                {
                    // 从 PIDL 创建 KnownFolderNative 对象
                    knownFolderNative = KnownFolderHelper.FromPIDL(base.PIDL);
                }

                // 断言 knownFolderNative 不为空
                Debug.Assert(knownFolderNative != null);
            }

            // 延迟初始化 knownFolderSettings 对象
            knownFolderSettings ??= new KnownFolderSettings(knownFolderNative!);

            // 返回 KnownFolderSettings 对象
            return knownFolderSettings;
        }
    }

    // 重写 Dispose 方法，处理资源释放
    protected override void Dispose(bool disposing)
    {
        # 如果处于释放状态，设置已知文件夹设置为 null
        if (disposing)
        {
            knownFolderSettings = null!;
        }

        # 如果已知文件夹的本地对象不为空，释放 COM 对象并将其设置为 null
        if (knownFolderNative != null)
        {
            Marshal.ReleaseComObject(knownFolderNative);
            knownFolderNative = null!;
        }

        # 调用基类的 Dispose 方法，传递释放标志
        base.Dispose(disposing);
    }
# 代码块结束符，表示当前代码段的结束
}
```