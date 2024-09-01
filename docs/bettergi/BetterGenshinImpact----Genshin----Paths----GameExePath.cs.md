# `.\better-genshin-impact\BetterGenshinImpact\Genshin\Paths\GameExePath.cs`

```cs
﻿using BetterGenshinImpact.GameTask.Common;  // 引入 BetterGenshinImpact.GameTask.Common 命名空间
using Microsoft.Extensions.Logging;  // 引入 Microsoft.Extensions.Logging 命名空间
using Microsoft.Win32;  // 引入 Microsoft.Win32 命名空间
using System;  // 引入 System 命名空间
using System.Collections.Frozen;  // 引入 System.Collections.Frozen 命名空间
using System.IO;  // 引入 System.IO 命名空间
using System.Linq;  // 引入 System.Linq 命名空间
using System.Text.RegularExpressions;  // 引入 System.Text.RegularExpressions 命名空间

namespace BetterGenshinImpact.Genshin.Paths;  // 定义 BetterGenshinImpact.Genshin.Paths 命名空间

internal partial class GameExePath  // 定义内部部分类 GameExePath
{
    // 定义静态只读的 FrozenSet 类型的 GameRegistryPaths 字段，包含游戏在注册表中的可能路径
    public static readonly FrozenSet<string> GameRegistryPaths = FrozenSet.ToFrozenSet(
    [
        @"HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Uninstall\原神",  // 注册表路径：原神
        @"HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Uninstall\Genshin Impact",  // 注册表路径：Genshin Impact
        // @"HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Uninstall\云·原神",  // 注册表路径：云·原神（已注释）
    ]);

    /// <summary>
    /// 获取游戏路径（不包括云原神）
    /// </summary>
    public static string? GetWithoutCloud()  // 公共静态方法，获取游戏路径（不包括云原神）
    {
        // 从 GameRegistryPaths 中选择不为空的第一个游戏可执行文件路径
        return GameRegistryPaths.Select(regKey => GetGameExePathFromRegistry(regKey, false)).FirstOrDefault(exePath => !string.IsNullOrEmpty(exePath));
    }

    /// <summary>
    /// 从注册表中查找游戏路径
    /// </summary>
    /// <param name="key">注册表键路径</param>
    /// <param name="isCloud">是否为云原神</param>
    /// <returns>游戏可执行文件的路径</returns>
    private static string? GetGameExePathFromRegistry(string key, bool isCloud)  // 私有静态方法，从注册表查找游戏路径
    {
        try
        {
            // 获取注册表中指定键的 InstallPath 值作为字符串
            var launcherPath = Registry.GetValue(key, "InstallPath", null) as string;
            if (isCloud)  // 如果是云原神
            {
                // 获取注册表中指定键的 ExeName 值作为字符串
                var exeName = Registry.GetValue(key, "ExeName", null) as string;
                // 合并 InstallPath 和 ExeName 形成可执行文件路径
                var exePath = Path.Join(launcherPath, exeName);
                // 检查可执行文件是否存在
                if (File.Exists(exePath))
                {
                    return exePath;  // 返回可执行文件路径
                }
            }
            else  // 如果不是云原神
            {
                // 合并 InstallPath 和 "config.ini" 形成配置文件路径
                var configPath = Path.Join(launcherPath, "config.ini");
                // 检查配置文件是否存在
                if (File.Exists(configPath))
                {
                    // 读取配置文件内容
                    var str = File.ReadAllText(configPath);
                    // 从配置文件中提取游戏安装路径
                    var installPath = GameInstallPathRegex().Match(str).Groups[1].Value.Trim();
                    // 从配置文件中提取游戏启动名称
                    var exeName = GameStartNameRegex().Match(str).Groups[1].Value.Trim();
                    // 合成完整的可执行文件路径
                    var exePath = Path.GetFullPath(exeName, installPath);
                    // 检查可执行文件是否存在
                    if (File.Exists(exePath))
                    {
                        return exePath;  // 返回可执行文件路径
                    }
                }
            }
        }
        catch (Exception e)  // 捕捉异常
        {
            // 记录警告日志，指示从注册表和启动器查找游戏路径失败
            TaskControl.Logger.LogWarning(e, "从注册表和启动器查找游戏路径失败");
        }

        return null;  // 如果找不到路径，返回 null
    }

    [GeneratedRegex(@"game_install_path=(.+)")]  // 定义用于提取游戏安装路径的正则表达式
    private static partial Regex GameInstallPathRegex();  // 定义 GameInstallPathRegex 方法

    [GeneratedRegex(@"game_start_name=(.+)")]  // 定义用于提取游戏启动名称的正则表达式
    private static partial Regex GameStartNameRegex();  // 定义 GameStartNameRegex 方法
}
```