# `.\better-genshin-impact\Build\MicaSetup\Natives\Shell\Dialogs\Interop\KnownFolders\KnownFoldersNativeMethods.cs`

```cs
# 引入必要的命名空间和库
﻿using System;
using System.Runtime.InteropServices;
using System.Security;

# 定义命名空间
namespace MicaSetup.Shell.Dialogs;

# 禁用对未托管代码的安全检查
[SuppressUnmanagedCodeSecurity]
# 定义内部静态类，用于管理已知文件夹的安全本机方法
internal static class KnownFoldersSafeNativeMethods
{
    # 定义结构体，布局为顺序布局，用于存储文件夹定义的信息
    [StructLayout(LayoutKind.Sequential)]
    internal struct NativeFolderDefinition
    {
        # 定义文件夹分类
        internal FolderCategory category;
        # 定义文件夹名称的指针
        internal nint name;
        # 定义文件夹描述的指针
        internal nint description;
        # 定义父文件夹的 GUID
        internal Guid parentId;
        # 定义相对路径的指针
        internal nint relativePath;
        # 定义解析名称的指针
        internal nint parsingName;
        # 定义工具提示的指针
        internal nint tooltip;
        # 定义本地化名称的指针
        internal nint localizedName;
        # 定义图标的指针
        internal nint icon;
        # 定义安全信息的指针
        internal nint security;
        # 定义文件夹属性
        internal uint attributes;
        # 定义定义选项
        internal DefinitionOptions definitionOptions;
        # 定义文件夹类型的 GUID
        internal Guid folderTypeId;
    }
}
```