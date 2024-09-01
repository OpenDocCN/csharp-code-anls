# `.\better-genshin-impact\Build\MicaSetup\Natives\Shell\Dialogs\Common\ShellThumbnailEnums.cs`

```cs
namespace MicaSetup.Shell.Dialogs;
// 定义一个命名空间，包含与 Shell 对话框相关的内容

public enum ShellThumbnailFormatOption
{
    // 枚举类型，用于指定缩略图格式选项
    Default, // 默认选项，通常表示系统的默认行为
    ThumbnailOnly = SIIGBF.ThumbnailOnly, // 仅获取缩略图
    IconOnly = SIIGBF.IconOnly, // 仅获取图标
}

public enum ShellThumbnailRetrievalOption
{
    // 枚举类型，用于指定缩略图检索选项
    Default, // 默认选项，通常表示系统的默认行为
    CacheOnly = SIIGBF.InCacheOnly, // 仅从缓存中检索
    MemoryOnly = SIIGBF.MemoryOnly, // 仅从内存中检索
}
```