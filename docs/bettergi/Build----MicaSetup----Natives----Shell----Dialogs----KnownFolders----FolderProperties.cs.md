# `.\better-genshin-impact\Build\MicaSetup\Natives\Shell\Dialogs\KnownFolders\FolderProperties.cs`

```cs
# 使用 .NET 的命名空间和库
using System;
using System.Runtime.InteropServices;
using System.Windows.Media.Imaging;

# 定义 MicaSetup.Shell.Dialogs 命名空间
namespace MicaSetup.Shell.Dialogs;

# 指定结构体的内存布局为顺序
[StructLayout(LayoutKind.Sequential)]
# 定义内部结构体 FolderProperties
internal struct FolderProperties
{
    # 文件夹名称
    internal string name;
    # 文件夹类别
    internal FolderCategory category;
    # 文件夹的规范名称
    internal string canonicalName;
    # 文件夹描述
    internal string description;
    # 父文件夹的唯一标识符
    internal Guid parentId;
    # 父文件夹的名称
    internal string parent;
    # 文件夹的相对路径
    internal string relativePath;
    # 文件夹的解析名称
    internal string parsingName;
    # 文件夹的工具提示资源 ID
    internal string tooltipResourceId;
    # 文件夹的工具提示文本
    internal string tooltip;
    # 文件夹的本地化名称
    internal string localizedName;
    # 文件夹的本地化名称资源 ID
    internal string localizedNameResourceId;
    # 文件夹图标的资源 ID
    internal string iconResourceId;
    # 文件夹图标
    internal BitmapSource icon;
    # 文件夹定义选项
    internal DefinitionOptions definitionOptions;
    # 文件夹的文件属性
    internal System.IO.FileAttributes fileAttributes;
    # 文件夹类型的唯一标识符
    internal Guid folderTypeId;
    # 文件夹类型
    internal string folderType;
    # 文件夹的唯一标识符
    internal Guid folderId;
    # 文件夹的路径
    internal string path;
    # 路径是否存在
    internal bool pathExists;
    # 文件夹的重定向能力
    internal RedirectionCapability redirection;
    # 文件夹的安全性
    internal string security;
}
```