# `.\better-genshin-impact\Build\MicaSetup\Natives\Shell\Dialogs\Interop\Common\IntPtrExtensions.cs`

```cs
# 引用 System.Runtime.InteropServices 命名空间，以便使用与非托管代码互操作相关的功能
﻿using System.Runtime.InteropServices;

# 定义一个命名空间，包含用于 MicaSetup 的对话框相关功能
namespace MicaSetup.Shell.Dialogs;

# 定义一个内部静态类，用于扩展 IntPtr 的功能
internal static class IntPtrExtensions
{
    # 定义一个扩展方法，将 nint 类型的指针转换为指定类型的对象
    public static T MarshalAs<T>(this nint ptr) => (T)Marshal.PtrToStructure(ptr, typeof(T));
    # Marshal.PtrToStructure 方法用于将非托管内存中的数据转换为托管对象
}
```