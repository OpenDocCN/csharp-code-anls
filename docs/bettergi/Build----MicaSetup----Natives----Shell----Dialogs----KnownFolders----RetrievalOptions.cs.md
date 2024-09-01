# `.\better-genshin-impact\Build\MicaSetup\Natives\Shell\Dialogs\KnownFolders\RetrievalOptions.cs`

```cs
# 定义一个内部枚举，用于表示检索选项
internal enum RetrievalOptions
{
    # 无选项，值为 0
    None = 0,
    # 创建选项，值为 0x00008000
    Create = 0x00008000,
    # 不验证选项，值为 0x00004000
    DontVerify = 0x00004000,
    # 不展开选项，值为 0x00002000
    DontUnexpand = 0x00002000,
    # 不使用别名选项，值为 0x00001000
    NoAlias = 0x00001000,
    # 初始化选项，值为 0x00000800
    Init = 0x00000800,
    # 默认路径选项，值为 0x00000400
    DefaultPath = 0x00000400,
    # 不是父级相对路径选项，值为 0x00000200
    NotParentRelative = 0x00000200,
}
```