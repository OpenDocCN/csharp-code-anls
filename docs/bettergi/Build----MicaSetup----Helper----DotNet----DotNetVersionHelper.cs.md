# `.\better-genshin-impact\Build\MicaSetup\Helper\DotNet\DotNetVersionHelper.cs`

```cs
﻿using Microsoft.Win32; // 引入 Microsoft.Win32 命名空间以访问注册表
using System; // 引入 System 命名空间以使用基础类型和功能

namespace MicaSetup.Helper // 定义一个名为 MicaSetup.Helper 的命名空间
{
    /// <summary>
    /// https://learn.microsoft.com/en-us/dotnet/framework/migration-guide/how-to-determine-which-versions-are-installed
    /// </summary>
    // 定义一个静态类 DotNetVersionHelper，帮助获取 .NET Framework 的版本信息
    public static class DotNetVersionHelper
    {
        // 定义一个静态方法 GetNet4xVersion，返回安装的 .NET Framework 4.x 的版本
        public static Version? GetNet4xVersion()
        {
            // 打开注册表中的 .NET Framework 4.x 完整版本的子项
            using RegistryKey key = Registry.LocalMachine.OpenSubKey(@"SOFTWARE\Microsoft\NET Framework Setup\NDP\v4\Full");

            // 如果注册表项存在
            if (key != null)
            {
                // 获取 "Install" 值，判断是否安装
                object? install = key.GetValue("Install");

                // 如果 "Install" 值存在且等于 1，表示安装了 .NET Framework 4.x
                if (install != null && ((int)install) == 1)
                {
                    // 获取 "Version" 值，表示版本号
                    object? version = key.GetValue("Version");

                    // 如果 "Version" 值是字符串
                    if (version is string versionString)
                    {
                        // 获取 "Release" 值，表示发行版本号
                        object? release = key.GetValue("Release");

                        // 如果 "Release" 值是整数
                        if (release is int releaseValue)
                        {
                            // 根据 "Release" 版本号返回具体的 .NET Framework 版本
                            if (releaseValue >= 533320)
                            {
                                return new Version(4, 8, 1); // 返回 .NET Framework 4.8.1
                            }
                            else if (releaseValue >= 528040)
                            {
                                return new Version(4, 8, 0); // 返回 .NET Framework 4.8.0
                            }
                            else if (releaseValue >= 461808)
                            {
                                return new Version(4, 7, 2); // 返回 .NET Framework 4.7.2
                            }
                            else if (releaseValue >= 461308)
                            {
                                return new Version(4, 7, 1); // 返回 .NET Framework 4.7.1
                            }
                            else if (releaseValue >= 460798)
                            {
                                return new Version(4, 7, 0); // 返回 .NET Framework 4.7.0
                            }
                            else if (releaseValue >= 394802)
                            {
                                return new Version(4, 6, 2); // 返回 .NET Framework 4.6.2
                            }
                            else if (releaseValue >= 394254)
                            {
                                return new Version(4, 6, 1); // 返回 .NET Framework 4.6.1
                            }
                            else if (releaseValue >= 393295)
                            {
                                return new Version(4, 6, 0); // 返回 .NET Framework 4.6.0
                            }
                            else if (releaseValue >= 379893)
                            {
                                return new Version(4, 5, 2); // 返回 .NET Framework 4.5.2
                            }
                            else if (releaseValue >= 378675)
                            {
                                return new Version(4, 5, 1); // 返回 .NET Framework 4.5.1
                            }
                            else if (releaseValue >= 378389)
                            {
                                return new Version(4, 5, 0); // 返回 .NET Framework 4.5.0
                            }
                        }
                        // 如果 "Release" 值不存在或不是整数，则返回 "Version" 字符串表示的版本
                        return new Version(versionString);
                    }
                }
                // 如果 "Install" 值不存在或不等于 1，则返回默认版本 4.5
                return new Version(4, 5);
            }
            // 如果注册表项不存在，则返回 null
            return null!;
        }

        // 定义一个静态方法 GetNet3xVersion，返回安装的 .NET Framework 3.x 的版本
        public static Version? GetNet3xVersion()
    {
        // 获取 .NET Framework 3.5 版本信息
        static Version? GetNet35Version()
        {
            // 打开注册表键，以检查 .NET Framework 3.5 的安装状态
            using RegistryKey key = Registry.LocalMachine.OpenSubKey(@"SOFTWARE\Microsoft\NET Framework Setup\NDP\v3.5");

            // 如果注册表键存在
            if (key != null)
            {
                // 获取安装状态
                object? install = key.GetValue("Install");

                // 如果安装状态存在且等于 1
                if (install != null && ((int)install) == 1)
                {
                    // 获取版本号
                    object? version = key.GetValue("Version");

                    // 如果版本号是字符串类型
                    if (version is string versionString)
                    {
                        // 返回新的 Version 对象，使用获取到的版本字符串
                        return new Version(versionString);
                    }
                }
                // 默认返回 .NET Framework 3.5 的版本
                return new Version(3, 5);
            }
            // 如果注册表键不存在，返回 null
            return null!;
        }

        // 获取 .NET Framework 3.0 版本信息
        static Version? GetNet30Version()
        {
            RegistryKey key = null!;

            try
            {
                // 尝试打开注册表键以检查 .NET Framework 3.0 SP2 的安装状态
                key = Registry.LocalMachine.OpenSubKey(@"SOFTWARE\Microsoft\NET Framework Setup\NDP\v3.0\SP2");
                // 如果 SP2 的键不存在，尝试打开 SP1 和 Setup 键
                key ??= Registry.LocalMachine.OpenSubKey(@"SOFTWARE\Microsoft\NET Framework Setup\NDP\v3.0\SP1");
                key ??= Registry.LocalMachine.OpenSubKey(@"SOFTWARE\Microsoft\NET Framework Setup\NDP\v3.0\Setup");

                // 如果注册表键存在
                if (key != null)
                {
                    // 获取安装成功状态
                    object? install = key.GetValue("InstallSuccess");

                    // 如果安装成功状态存在且等于 1
                    if (install != null && ((int)install) == 1)
                    {
                        // 获取版本号
                        object? version = key.GetValue("Version");

                        // 如果版本号是字符串类型
                        if (version is string versionString)
                        {
                            // 返回新的 Version 对象，使用获取到的版本字符串
                            return new Version(versionString);
                        }
                    }
                    // 默认返回 .NET Framework 3.0 的版本
                    return new Version(3, 0);
                }
                // 如果注册表键不存在，返回 null
                return null!;
            }
            // 确保最终关闭注册表键
            finally
            {
                key?.Dispose();
            }
        }

        // 获取 .NET Framework 3.5 的版本
        Version? version = GetNet35Version();
        // 如果版本不为空，则返回该版本
        if (version != null)
        {
            return version;
        }

        // 获取 .NET Framework 3.0 的版本
        version = GetNet30Version();
        // 如果版本不为空，则返回该版本
        if (version != null)
        {
            return version;
        }
        // 如果没有找到任何版本，返回 null
        return null!;
    }

    // 获取 .NET Framework 2.x 的版本（方法尚未实现）
    public static Version? GetNet2xVersion()
    {
        RegistryKey key = null!; // 声明一个 RegistryKey 类型的变量 key，初始化为 null
    
        try
        {
            // 尝试打开注册表项，路径为 v2.0.50727 的 SP2，获取 NET Framework 2.0.50727 SP2 的注册表项
            key = Registry.LocalMachine.OpenSubKey(@"SOFTWARE\Microsoft\NET Framework Setup\NDP\v2.0.50727\SP2");
            // 如果 SP2 的注册表项为空，则尝试打开 SP1 的注册表项
            key ??= Registry.LocalMachine.OpenSubKey(@"SOFTWARE\Microsoft\NET Framework Setup\NDP\v2.0.50727\SP1");
            // 如果 SP1 的注册表项为空，则尝试打开 Setup 的注册表项
            key ??= Registry.LocalMachine.OpenSubKey(@"SOFTWARE\Microsoft\NET Framework Setup\NDP\v2.0.50727\Setup");
    
            if (key != null) // 如果找到注册表项
            {
                // 获取 "Install" 的值，用于检查是否安装了 .NET Framework
                object? install = key.GetValue("Install");
    
                if (install != null && ((int)install) == 1) // 如果 "Install" 值不为空且等于 1
                {
                    // 获取 "Version" 的值
                    object? version = key.GetValue("Version");
    
                    if (version is string versionString) // 如果 "Version" 的值是字符串类型
                    {
                        // 将版本字符串转换为 Version 对象并返回
                        return new Version(versionString);
                    }
                }
                // 如果没有找到有效的安装版本，返回默认版本 2.0.50727
                return new Version(2, 0, 50727);
            }
            // 如果没有找到注册表项，返回 null
            return null!;
        }
        finally
        {
            // 确保无论如何都释放注册表项资源
            key?.Dispose();
        }
    }
    
    public static Version? GetNet1xVersion()
    {
        RegistryKey key = null!; // 声明一个 RegistryKey 类型的变量 key，初始化为 null
    
        try
        {
            // 尝试打开注册表项，路径为 v1.1.4322，获取 NET Framework 1.1.4322 的注册表项
            key = Registry.LocalMachine.OpenSubKey(@"SOFTWARE\Microsoft\NET Framework Setup\NDP\v1.1.4322");
            if (key != null) // 如果找到注册表项
            {
                // 获取 "Install" 的值，用于检查是否安装了 .NET Framework
                object? install = key.GetValue("Install");
    
                if (install != null && ((int)install) == 1) // 如果 "Install" 值不为空且等于 1
                {
                    // 获取 "Version" 的值
                    object? version = key.GetValue("Version");
    
                    if (version is string versionString) // 如果 "Version" 的值是字符串类型
                    {
                        // 将版本字符串转换为 Version 对象并返回
                        return new Version(versionString);
                    }
                }
                // 如果没有找到有效的安装版本，返回默认版本 1.1.4322
                return new Version(1, 1, 4322);
            }
    
            // 如果 v1.1.4322 的注册表项为空，尝试打开 v1.0 的注册表项
            key ??= Registry.LocalMachine.OpenSubKey(@"SOFTWARE\Microsoft\NET Framework Setup\NDP\v1.0");
            if (key != null) // 如果找到注册表项
            {
                // 获取 "Install" 的值，用于检查是否安装了 .NET Framework
                object? install = key.GetValue("Install");
    
                if (install != null && ((int)install) == 1) // 如果 "Install" 值不为空且等于 1
                {
                    // 获取 "Version" 的值
                    object? version = key.GetValue("Version");
    
                    if (version is string versionString) // 如果 "Version" 的值是字符串类型
                    {
                        // 将版本字符串转换为 Version 对象并返回
                        return new Version(versionString);
                    }
                }
                // 如果没有找到有效的安装版本，返回默认版本 1.0
                return new Version(1, 0);
            }
    
            // 如果没有找到注册表项，返回 null
            return null!;
        }
        finally
        {
            // 确保无论如何都释放注册表项资源
            key?.Dispose();
        }
    }
# 这是一个关闭大括号，通常用于表示一个代码块的结束
}
```