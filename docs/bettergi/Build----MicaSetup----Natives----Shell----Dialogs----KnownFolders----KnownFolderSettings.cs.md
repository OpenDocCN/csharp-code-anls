# `.\better-genshin-impact\Build\MicaSetup\Natives\Shell\Dialogs\KnownFolders\KnownFolderSettings.cs`

```cs
# 引入 MicaSetup.Natives 命名空间中的内容
﻿using MicaSetup.Natives;
# 引入 System 命名空间中的内容
using System;
# 引入 System.Diagnostics 命名空间中的内容
using System.Diagnostics;
# 引入 System.Runtime.InteropServices 命名空间中的内容
using System.Runtime.InteropServices;

# 定义 MicaSetup.Shell.Dialogs 命名空间
namespace MicaSetup.Shell.Dialogs;

# 定义一个内部类 KnownFolderSettings
internal class KnownFolderSettings
{
    # 定义一个私有字段，用于存储文件夹属性
    private FolderProperties knownFolderProperties;

    # 构造函数，接收 IKnownFolderNative 对象，并调用 GetFolderProperties 方法初始化文件夹属性
    internal KnownFolderSettings(IKnownFolderNative knownFolderNative) => GetFolderProperties(knownFolderNative);

    # 公开属性，返回文件夹的规范名称
    public string CanonicalName => knownFolderProperties.canonicalName;

    # 公开属性，返回文件夹的类别
    public FolderCategory Category => knownFolderProperties.category;

    # 公开属性，返回文件夹的定义选项
    public DefinitionOptions DefinitionOptions => knownFolderProperties.definitionOptions;

    # 公开属性，返回文件夹的描述
    public string Description => knownFolderProperties.description;

    # 公开属性，返回文件夹的唯一标识符
    public Guid FolderId => knownFolderProperties.folderId;

    # 公开属性，返回文件夹的类型
    public string FolderType => knownFolderProperties.folderType;

    # 公开属性，返回文件夹类型的唯一标识符
    public Guid FolderTypeId => knownFolderProperties.folderTypeId;

    # 公开属性，返回文件夹的本地化名称
    public string LocalizedName => knownFolderProperties.localizedName;

    # 公开属性，返回文件夹本地化名称的资源 ID
    public string LocalizedNameResourceId => knownFolderProperties.localizedNameResourceId;

    # 公开属性，返回文件夹的父级 ID
    public Guid ParentId => knownFolderProperties.parentId;

    # 公开属性，返回文件夹的路径
    public string Path => knownFolderProperties.path;

    # 公开属性，返回路径是否存在的布尔值
    public bool PathExists => knownFolderProperties.pathExists;

    # 公开属性，返回文件夹的重定向能力
    public RedirectionCapability Redirection => knownFolderProperties.redirection;

    # 公开属性，返回文件夹的相对路径
    public string RelativePath => knownFolderProperties.relativePath;

    # 公开属性，返回文件夹的安全性
    public string Security => knownFolderProperties.security;

    # 公开属性，返回文件夹的工具提示
    public string Tooltip => knownFolderProperties.tooltip;

    # 公开属性，返回工具提示的资源 ID
    public string TooltipResourceId => knownFolderProperties.tooltipResourceId;

    # 公开属性，返回文件夹的文件属性
    public System.IO.FileAttributes FileAttributes => knownFolderProperties.fileAttributes;

    # 定义一个私有方法，接收 IKnownFolderNative 对象并初始化文件夹属性
    private void GetFolderProperties(IKnownFolderNative knownFolderNative)
    # 确保 knownFolderNative 对象不为 null，否则断言失败
    Debug.Assert(knownFolderNative != null);

    # 从 knownFolderNative 获取文件夹定义，输出到 nativeFolderDefinition 变量
    knownFolderNative!.GetFolderDefinition(out var nativeFolderDefinition);

    try
    {
        # 将 nativeFolderDefinition 的各个字段赋值给 knownFolderProperties 对象的对应属性
        knownFolderProperties.category = nativeFolderDefinition.category;
        knownFolderProperties.canonicalName = Marshal.PtrToStringUni(nativeFolderDefinition.name);
        knownFolderProperties.description = Marshal.PtrToStringUni(nativeFolderDefinition.description);
        knownFolderProperties.parentId = nativeFolderDefinition.parentId;
        knownFolderProperties.relativePath = Marshal.PtrToStringUni(nativeFolderDefinition.relativePath);
        knownFolderProperties.parsingName = Marshal.PtrToStringUni(nativeFolderDefinition.parsingName);
        knownFolderProperties.tooltipResourceId = Marshal.PtrToStringUni(nativeFolderDefinition.tooltip);
        knownFolderProperties.localizedNameResourceId = Marshal.PtrToStringUni(nativeFolderDefinition.localizedName);
        knownFolderProperties.iconResourceId = Marshal.PtrToStringUni(nativeFolderDefinition.icon);
        knownFolderProperties.security = Marshal.PtrToStringUni(nativeFolderDefinition.security);
        knownFolderProperties.fileAttributes = (System.IO.FileAttributes)nativeFolderDefinition.attributes;
        knownFolderProperties.definitionOptions = nativeFolderDefinition.definitionOptions;
        knownFolderProperties.folderTypeId = nativeFolderDefinition.folderTypeId;
        # 从文件夹类型 ID 获取文件夹类型
        knownFolderProperties.folderType = FolderTypes.GetFolderType(knownFolderProperties.folderTypeId);

        # 获取文件夹路径及路径是否存在，输出到 path 和 pathExists 变量
        knownFolderProperties.path = GetPath(out var pathExists, knownFolderNative);
        knownFolderProperties.pathExists = pathExists;

        # 获取文件夹的重定向能力
        knownFolderProperties.redirection = knownFolderNative.GetRedirectionCapabilities();

        # 从资源 ID 获取提示信息和本地化名称
        knownFolderProperties.tooltip = NativeMethods.GetStringResource(knownFolderProperties.tooltipResourceId);
        knownFolderProperties.localizedName = NativeMethods.GetStringResource(knownFolderProperties.localizedNameResourceId);

        # 获取文件夹 ID
        knownFolderProperties.folderId = knownFolderNative.GetId();
    }
    finally
    {
        # 释放 nativeFolderDefinition 中分配的内存
        Marshal.FreeCoTaskMem(nativeFolderDefinition.name);
        Marshal.FreeCoTaskMem(nativeFolderDefinition.description);
        Marshal.FreeCoTaskMem(nativeFolderDefinition.relativePath);
        Marshal.FreeCoTaskMem(nativeFolderDefinition.parsingName);
        Marshal.FreeCoTaskMem(nativeFolderDefinition.tooltip);
        Marshal.FreeCoTaskMem(nativeFolderDefinition.localizedName);
        Marshal.FreeCoTaskMem(nativeFolderDefinition.icon);
        Marshal.FreeCoTaskMem(nativeFolderDefinition.security);
    }
    
    # 获取文件夹路径，并输出路径是否存在的状态
    private string GetPath(out bool fileExists, IKnownFolderNative knownFolderNative)
    # 确保 knownFolderNative 不是 null
    Debug.Assert(knownFolderNative != null);

    # 初始化 kfPath 为空字符串
    var kfPath = string.Empty;
    # 初始化 fileExists 为 true，假设文件存在
    fileExists = true;

    # 如果 knownFolderProperties 的类别是虚拟文件夹
    if (knownFolderProperties.category == FolderCategory.Virtual)
    {
        # 将 fileExists 设置为 false，表示文件不存在
        fileExists = false;
        # 返回空的 kfPath
        return kfPath;
    }

    try
    {
        # 尝试获取 knownFolderNative 的路径
        kfPath = knownFolderNative!.GetPath(0);
    }
    catch (System.IO.FileNotFoundException)
    {
        # 如果文件未找到，将 fileExists 设置为 false
        fileExists = false;
    }
    catch (System.IO.DirectoryNotFoundException)
    {
        # 如果目录未找到，将 fileExists 设置为 false
        fileExists = false;
    }

    # 返回获取到的路径
    return kfPath;
# 这是一个闭合的大括号，标记了代码块的结束
}
```