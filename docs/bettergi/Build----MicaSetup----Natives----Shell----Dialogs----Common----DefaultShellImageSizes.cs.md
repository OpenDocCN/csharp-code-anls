# `.\better-genshin-impact\Build\MicaSetup\Natives\Shell\Dialogs\Common\DefaultShellImageSizes.cs`

```cs
# 定义一个包含默认图标尺寸的命名空间
namespace MicaSetup.Shell.Dialogs;

# 定义一个静态类，包含默认图标尺寸
public static class DefaultIconSize
{
    # 定义一个静态常量，表示超大图标尺寸
    public static readonly System.Windows.Size ExtraLarge = new System.Windows.Size(256, 256);
    # 定义一个静态常量，表示大图标尺寸
    public static readonly System.Windows.Size Large = new System.Windows.Size(48, 48);
    # 定义一个静态常量，表示最大图标尺寸，与 ExtraLarge 尺寸相同
    public static readonly System.Windows.Size Maximum = new System.Windows.Size(256, 256);
    # 定义一个静态常量，表示中等图标尺寸
    public static readonly System.Windows.Size Medium = new System.Windows.Size(32, 32);
    # 定义一个静态常量，表示小图标尺寸
    public static readonly System.Windows.Size Small = new System.Windows.Size(16, 16);
}

# 定义一个静态类，包含默认缩略图尺寸
public static class DefaultThumbnailSize
{
    # 定义一个静态常量，表示超大缩略图尺寸
    public static readonly System.Windows.Size ExtraLarge = new System.Windows.Size(1024, 1024);
    # 定义一个静态常量，表示大缩略图尺寸
    public static readonly System.Windows.Size Large = new System.Windows.Size(256, 256);
    # 定义一个静态常量，表示最大缩略图尺寸，与 ExtraLarge 尺寸相同
    public static readonly System.Windows.Size Maximum = new System.Windows.Size(1024, 1024);
    # 定义一个静态常量，表示中等缩略图尺寸
    public static readonly System.Windows.Size Medium = new System.Windows.Size(96, 96);
    # 定义一个静态常量，表示小缩略图尺寸
    public static readonly System.Windows.Size Small = new System.Windows.Size(32, 32);
}
```