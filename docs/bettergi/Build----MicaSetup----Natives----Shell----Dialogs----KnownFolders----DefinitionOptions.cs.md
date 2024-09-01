# `.\better-genshin-impact\Build\MicaSetup\Natives\Shell\Dialogs\KnownFolders\DefinitionOptions.cs`

```cs
# 使用系统命名空间
using System;

# 定义一个名为 MicaSetup.Shell.Dialogs 的命名空间
namespace MicaSetup.Shell.Dialogs;

# 定义一个带有标志特性的枚举类型 DefinitionOptions
[Flags]
public enum DefinitionOptions
{
    # 定义一个没有任何标志的选项
    None = 0x0,
    # 定义一个仅本地重定向的选项
    LocalRedirectOnly = 0x2,
    # 定义一个可以漫游的选项
    Roamable = 0x4,
    # 定义一个预创建的选项
    Precreate = 0x8,
}
```