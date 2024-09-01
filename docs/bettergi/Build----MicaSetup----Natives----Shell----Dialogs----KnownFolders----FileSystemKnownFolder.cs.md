# `.\better-genshin-impact\Build\MicaSetup\Natives\Shell\Dialogs\KnownFolders\FileSystemKnownFolder.cs`

```cs
# 允许 C# 使用文件系统已知文件夹的功能，并实现接口和资源管理
﻿using System;
using System.Diagnostics;
using System.Runtime.InteropServices;

namespace MicaSetup.Shell.Dialogs;

#pragma warning disable CS8618

# 文件系统已知文件夹类，继承自 ShellFileSystemFolder， 实现 IKnownFolder 和 IDisposable 接口
public class FileSystemKnownFolder : ShellFileSystemFolder, IKnownFolder, IDisposable
{
    # 存储已知文件夹的原生接口
    private IKnownFolderNative knownFolderNative;
    # 存储已知文件夹的设置
    private KnownFolderSettings knownFolderSettings;

    # 内部构造函数，使用 IShellItem2 初始化
    internal FileSystemKnownFolder(IShellItem2 shellItem) : base(shellItem)
    {
    }

    # 内部构造函数，使用 IKnownFolderNative 初始化
    internal FileSystemKnownFolder(IKnownFolderNative kf)
    {
        # 断言 kf 不为空
        Debug.Assert(kf != null);
        knownFolderNative = kf!;

        # 创建 Guid 实例用于获取 ShellItem
        var guid = new Guid(ShellIIDGuid.IShellItem2);
        # 从已知文件夹获取 ShellItem
        knownFolderNative!.GetShellItem(0, ref guid, out nativeShellItem);
    }

    # 获取已知文件夹的规范名称
    public string CanonicalName => KnownFolderSettings.CanonicalName;

    # 获取已知文件夹的类别
    public FolderCategory Category => KnownFolderSettings.Category;

    # 获取已知文件夹的定义选项
    public DefinitionOptions DefinitionOptions => KnownFolderSettings.DefinitionOptions;

    # 获取已知文件夹的描述
    public string Description => KnownFolderSettings.Description;

    # 获取已知文件夹的文件属性
    public System.IO.FileAttributes FileAttributes => KnownFolderSettings.FileAttributes;

    # 获取已知文件夹的 GUID
    public Guid FolderId => KnownFolderSettings.FolderId;

    # 获取已知文件夹的类型
    public string FolderType => KnownFolderSettings.FolderType;

    # 获取已知文件夹的类型 GUID
    public Guid FolderTypeId => KnownFolderSettings.FolderTypeId;

    # 获取已知文件夹的本地化名称
    public string LocalizedName => KnownFolderSettings.LocalizedName;

    # 获取已知文件夹的本地化名称资源 ID
    public string LocalizedNameResourceId => KnownFolderSettings.LocalizedNameResourceId;

    # 获取已知文件夹的父级 ID
    public Guid ParentId => KnownFolderSettings.ParentId;

    # 重写父类的 ParsingName 属性
    public override string ParsingName => base.ParsingName;

    # 重写父类的 Path 属性
    public override string Path => KnownFolderSettings.Path;

    # 获取已知文件夹路径是否存在的标志
    public bool PathExists => KnownFolderSettings.PathExists;

    # 获取已知文件夹的重定向能力
    public RedirectionCapability Redirection => KnownFolderSettings.Redirection;

    # 获取已知文件夹的相对路径
    public string RelativePath => KnownFolderSettings.RelativePath;

    # 获取已知文件夹的安全设置
    public string Security => KnownFolderSettings.Security;

    # 获取已知文件夹的工具提示
    public string Tooltip => KnownFolderSettings.Tooltip;

    # 获取已知文件夹的工具提示资源 ID
    public string TooltipResourceId => KnownFolderSettings.TooltipResourceId;

    # 获取已知文件夹设置的封装属性
    private KnownFolderSettings KnownFolderSettings
    {
        get
        {
            # 如果 knownFolderNative 为 null
            if (knownFolderNative == null)
            {
                # 如果 nativeShellItem 不为空且 PIDL 为 0
                if (nativeShellItem != null && base.PIDL == 0)
                {
                    # 从 ShellItem 获取 PIDL
                    base.PIDL = ShellHelper.PidlFromShellItem(nativeShellItem);
                }

                # 如果 PIDL 不为 0
                if (base.PIDL != 0)
                {
                    # 从 PIDL 获取已知文件夹帮助对象
                    knownFolderNative = KnownFolderHelper.FromPIDL(base.PIDL);
                }

                # 断言 knownFolderNative 不为空
                Debug.Assert(knownFolderNative != null);
            }

            # 如果 knownFolderSettings 为 null，初始化它
            knownFolderSettings ??= new KnownFolderSettings(knownFolderNative!);

            # 返回已知文件夹设置
            return knownFolderSettings;
        }
    }

    # 重写 Dispose 方法以进行资源清理
    protected override void Dispose(bool disposing)
    # 检查是否正在释放资源
    {
        if (disposing)
        {
            # 释放已知文件夹设置的引用
            knownFolderSettings = null!;
        }

        # 检查是否存在已知文件夹的本地对象
        if (knownFolderNative != null)
        {
            # 释放 COM 对象并将其设置为 null
            Marshal.ReleaseComObject(knownFolderNative);
            knownFolderNative = null!;
        }

        # 调用基类的 Dispose 方法，传递 disposing 标志
        base.Dispose(disposing);
    }
# 终止当前代码块，标识代码的结束
}
```