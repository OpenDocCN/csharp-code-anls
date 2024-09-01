# `.\better-genshin-impact\Build\MicaSetup\Helper\System\RegistyAutoRunHelper.cs`

```cs
# 引入 Microsoft.Win32 命名空间以访问注册表功能
﻿using Microsoft.Win32;

# 定义 MicaSetup.Helper 命名空间
namespace MicaSetup.Helper;

# 定义静态类 RegistyAutoRunHelper 用于操作注册表中的自动启动项
public static class RegistyAutoRunHelper
{
    # 定义注册表中存储启动项位置的常量
    private const string RunLocation = @"Software\Microsoft\Windows\CurrentVersion\Run";

    # 定义静态方法 Enable，用于在注册表中添加启动项
    public static void Enable(string keyName, string launchCommand)
    {
        # 使用当前用户的注册表创建或打开指定的子项
        using RegistryKey key = Registry.CurrentUser.CreateSubKey(RunLocation);
        # 将启动项名称和命令写入注册表
        key?.SetValue(keyName, launchCommand);
    }

    # 定义静态方法 IsEnabled，用于检查指定的启动项是否已启用
    public static bool IsEnabled(string keyName, string launchCommand)
    {
        # 使用当前用户的注册表打开指定的子项
        using RegistryKey key = Registry.CurrentUser.OpenSubKey(RunLocation);

        # 如果指定的子项不存在，返回 false
        if (key == null)
        {
            return false;
        }

        # 获取指定启动项的值
        string value = (string)key.GetValue(keyName);

        # 如果启动项的值不存在，返回 false
        if (value == null)
        {
            return false;
        }

        # 返回启动项的值是否与提供的命令匹配
        return value == launchCommand;
    }

    # 定义静态方法 Disable，用于从注册表中删除启动项
    public static void Disable(string keyName, string launchCommand = null!)
    {
        # 使用当前用户的注册表创建或打开指定的子项
        using RegistryKey key = Registry.CurrentUser.CreateSubKey(RunLocation);

        # 不使用 launchCommand 参数
        _ = launchCommand;
        # 如果指定的子项不存在，直接返回
        if (key == null)
        {
            return;
        }

        # 如果指定的启动项存在，则从注册表中删除
        if (key.GetValue(keyName) != null)
        {
            key.DeleteValue(keyName);
        }
    }

    # 定义静态方法 SetEnabled，根据布尔值启用或禁用启动项
    public static void SetEnabled(bool enable, string keyName, string launchCommand)
    {
        # 如果启用标志为 true，则调用 Enable 方法；否则调用 Disable 方法
        if (enable)
        {
            Enable(keyName, launchCommand);
        }
        else
        {
            Disable(keyName);
        }
    }
}
```