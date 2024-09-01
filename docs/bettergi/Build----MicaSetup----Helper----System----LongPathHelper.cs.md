# `.\better-genshin-impact\Build\MicaSetup\Helper\System\LongPathHelper.cs`

```cs
# 引入 MicaSetup.Natives 命名空间，可能包含与本地操作系统相关的功能
﻿using MicaSetup.Natives;
# 引入 Microsoft.Win32 命名空间，提供与 Windows 注册表相关的功能
using Microsoft.Win32;

# 定义一个命名空间 MicaSetup.Helper
namespace MicaSetup.Helper;

# 定义一个静态类 LongPathHelper
public static class LongPathHelper
{
    # 说明：更改计算机配置，以允许程序绕过 260 个字符的 "MAX_PATH" 限制
    /// <summary>
    /// Changes your machine configuration to allow programs, to pass the 260 character "MAX_PATH" limitation.
    /// </summary>
    # 启用长路径支持，传入路径以设置 DLL 目录
    public static bool EnableLongPath(string path)
    {
        # 调用 Kernel32.SetDllDirectory 方法，将路径前缀加上 @"\\?\" 来启用长路径
        return Kernel32.SetDllDirectory(@"\\?\" + path);
    }

    # 说明：更改计算机配置，以允许程序绕过 260 个字符的 "MAX_PATH" 限制
    /// <summary>
    /// Changes your machine configuration to allow programs, to pass the 260 character "MAX_PATH" limitation.
    /// </summary>
    # 启用长路径支持，修改注册表设置
    public static void EnableLongPath()
    {
        # 打开注册表路径 @"SYSTEM\CurrentControlSet\Control\FileSystem" 并以可写模式访问
        using RegistryKey key = Registry.LocalMachine.OpenSubKey(@"SYSTEM\CurrentControlSet\Control\FileSystem", true);
        # 如果注册表项存在，设置 "LongPathsEnabled" 键为 1，表示启用长路径
        key?.SetValue("LongPathsEnabled", 1, RegistryValueKind.DWord);
    }
}
```