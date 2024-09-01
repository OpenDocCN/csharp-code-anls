# `.\better-genshin-impact\Build\MicaSetup\Natives\Shell\Dialogs\KnownFolders\IKnownFolder.cs`

```cs
# 定义一个接口 IKnownFolder，继承自 IDisposable 和 IEnumerable<ShellObject> 接口
public interface IKnownFolder : IDisposable, IEnumerable<ShellObject>
{
    # 获取文件夹的规范名称
    string CanonicalName { get; }

    # 获取文件夹的类别
    FolderCategory Category { get; }

    # 获取文件夹的定义选项
    DefinitionOptions DefinitionOptions { get; }

    # 获取文件夹的描述
    string Description { get; }

    # 获取文件夹的文件属性
    FileAttributes FileAttributes { get; }

    # 获取文件夹的 GUID 标识
    Guid FolderId { get; }

    # 获取文件夹的类型
    string FolderType { get; }

    # 获取文件夹类型的 GUID 标识
    Guid FolderTypeId { get; }

    # 获取文件夹的本地化名称
    string LocalizedName { get; }

    # 获取文件夹本地化名称的资源 ID
    string LocalizedNameResourceId { get; }

    # 获取文件夹父级的 GUID 标识
    Guid ParentId { get; }

    # 获取文件夹的解析名称
    string ParsingName { get; }

    # 获取文件夹的路径
    string Path { get; }

    # 判断文件夹路径是否存在
    bool PathExists { get; }

    # 获取文件夹的重定向能力
    RedirectionCapability Redirection { get; }

    # 获取文件夹的相对路径
    string RelativePath { get; }

    # 获取文件夹的安全信息
    string Security { get; }

    # 获取文件夹的工具提示
    string Tooltip { get; }

    # 获取文件夹工具提示的资源 ID
    string TooltipResourceId { get; }
}
```