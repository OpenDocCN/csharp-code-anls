# `.\better-genshin-impact\Build\MicaSetup\Natives\Shell\Dialogs\Interop\StockIcons\StockIconsNativeMethods.cs`

```cs
﻿using MicaSetup.Natives; // 引用自定义的本地方法库
using System; // 引用系统基础功能
using System.Runtime.InteropServices; // 引用用于与非托管代码交互的功能

namespace MicaSetup.Shell.Dialogs; // 定义命名空间

internal static class StockIconsNativeMethods
{
    // 定义枚举用于标识和配置库存图标的选项
    [Flags]
    internal enum StockIconOptions
    {
        Large = 0x000000000, // 大图标
        Small = 0x000000001, // 小图标
        ShellSize = 0x000000004, // 使用 Shell 的大小
        Handle = 0x000000100, // 图标的句柄
        SystemIndex = 0x000004000, // 系统图标索引
        LinkOverlay = 0x000008000, // 链接覆盖
        Selected = 0x000010000, // 选中状态
    }

    // 声明外部方法 SHGetStockIconInfo，用于获取库存图标的信息
    [PreserveSig]
    [DllImport(Lib.Shell32, CharSet = CharSet.Unicode,
    ExactSpelling = true, SetLastError = false)]
    internal static extern HResult SHGetStockIconInfo(
        StockIconIdentifier identifier, // 图标标识符
        StockIconOptions flags, // 图标选项
        ref StockIconInfo info); // 图标信息结构体的引用

    // 定义结构体，用于存储库存图标的信息
    [StructLayoutAttribute(LayoutKind.Sequential, CharSet = CharSet.Unicode)]
    internal struct StockIconInfo
    {
        internal uint StuctureSize; // 结构体大小
        internal nint Handle; // 图标的句柄
        internal int ImageIndex; // 图标的索引
        internal int Identifier; // 图标的标识符

        // 存储图标路径的字段，大小为 260 字节
        [MarshalAs(UnmanagedType.ByValTStr, SizeConst = 260)]
        internal string Path;
    }
}
```