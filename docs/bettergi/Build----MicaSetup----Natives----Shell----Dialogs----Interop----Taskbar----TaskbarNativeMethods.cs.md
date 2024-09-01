# `.\better-genshin-impact\Build\MicaSetup\Natives\Shell\Dialogs\Interop\Taskbar\TaskbarNativeMethods.cs`

```cs
# 定义缩略图透明度类型枚举
public enum ThumbnailAlphaType
{
    # 未知透明度类型
    Unknown = 0,

    # 无透明通道
    NoAlphaChannel = 1,

    # 有透明通道
    HasAlphaChannel = 2,
}

# 定义已知目标类别枚举
internal enum KnownDestinationCategory
{
    # 常用目标
    Frequent = 1,
    # 最近目标
    Recent
}

# 定义设置标签属性选项枚举
internal enum SetTabPropertiesOption
{
    # 无选项
    None = 0x0,
    # 始终使用应用程序缩略图
    UseAppThumbnailAlways = 0x1,
    # 激活时使用应用程序缩略图
    UseAppThumbnailWhenActive = 0x2,
    # 始终使用应用程序预览
    UseAppPeekAlways = 0x4,
    # 激活时使用应用程序预览
    UseAppPeekWhenActive = 0x8
}

# 定义任务栏进度条状态枚举
internal enum TaskbarProgressBarStatus
{
    # 无进度
    NoProgress = 0,
    # 不确定进度
    Indeterminate = 0x1,
    # 正常进度
    Normal = 0x2,
    # 错误进度
    Error = 0x4,
    # 暂停进度
    Paused = 0x8
}

# 定义缩略图按钮掩码枚举
internal enum ThumbButtonMask
{
    # 位图掩码
    Bitmap = 0x1,
    # 图标掩码
    Icon = 0x2,
    # 工具提示掩码
    Tooltip = 0x4,
    # 按钮标志掩码
    THB_FLAGS = 0x8
}

# 定义缩略图按钮选项枚举，具有标志属性
[Flags]
internal enum ThumbButtonOptions
{
    # 启用选项
    Enabled = 0x00000000,
    # 禁用选项
    Disabled = 0x00000001,
    # 点击时关闭选项
    DismissOnClick = 0x00000002,
    # 无背景选项
    NoBackground = 0x00000004,
    # 隐藏选项
    Hidden = 0x00000008,
    # 非交互式选项
    NonInteractive = 0x00000010
}

# 定义缩略图按钮结构体，按顺序布局
[StructLayout(LayoutKind.Sequential, CharSet = CharSet.Auto)]
internal struct ThumbButton
{
    # 按钮被点击的标识符
    internal const int Clicked = 0x1800;

    # 按钮掩码
    [MarshalAs(UnmanagedType.U4)]
    internal ThumbButtonMask Mask;

    # 按钮 ID
    internal uint Id;
    # 按钮位图
    internal uint Bitmap;
    # 按钮图标
    internal nint Icon;

    # 按钮工具提示文本
    [MarshalAs(UnmanagedType.ByValTStr, SizeConst = 260)]
    internal string Tip;

    # 按钮选项标志
    [MarshalAs(UnmanagedType.U4)]
    internal ThumbButtonOptions Flags;
}
```