# `.\better-genshin-impact\Build\MicaSetup\Natives\Kernel32.cs`

```cs
# 引入系统运行时库和安全相关功能
﻿using System.Runtime.InteropServices;
using System.Security;

# 定义一个静态类来封装对 Windows Kernel32 库的调用
namespace MicaSetup.Natives;

public static class Kernel32
{
    # 声明一个外部方法，用于从指定的 DLL 文件中加载库
    [DllImport(Lib.Kernel32, SetLastError = true, ThrowOnUnmappableChar = true, BestFitMapping = false)]
    [DefaultDllImportSearchPaths(DllImportSearchPath.System32)]
    public static extern nint LoadLibrary([MarshalAs(UnmanagedType.LPStr)] string fileName);

    # 声明一个外部方法，用于移动或重命名指定的文件
    [SecurityCritical]
    [DllImport(Lib.Kernel32, SetLastError = true, CharSet = CharSet.Unicode)]
    [DefaultDllImportSearchPaths(DllImportSearchPath.System32)]
    public static extern bool MoveFileEx(string lpExistingFileName, string lpNewFileName, MoveFileFlags dwFlags);

    # 声明一个外部方法，用于设置 DLL 搜索路径
    [SecurityCritical]
    [DllImport(Lib.Kernel32, CharSet = CharSet.Unicode, SetLastError = true)]
    [DefaultDllImportSearchPaths(DllImportSearchPath.System32)]
    public static extern bool SetDllDirectory(string lpPathName);
}
```