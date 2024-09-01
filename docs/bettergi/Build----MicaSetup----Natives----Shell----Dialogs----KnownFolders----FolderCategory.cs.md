# `.\better-genshin-impact\Build\MicaSetup\Natives\Shell\Dialogs\KnownFolders\FolderCategory.cs`

```cs
# 定义 MicaSetup.Shell.Dialogs 命名空间
namespace MicaSetup.Shell.Dialogs;

# 定义一个枚举类型 FolderCategory
public enum FolderCategory
{
    # 表示没有分类
    None = 0x00,
    # 表示虚拟文件夹
    Virtual = 0x1,
    # 表示固定文件夹
    Fixed = 0x2,
    # 表示公共文件夹
    Common = 0x3,
    # 表示每个用户特有的文件夹
    PerUser = 0x4,
}
```