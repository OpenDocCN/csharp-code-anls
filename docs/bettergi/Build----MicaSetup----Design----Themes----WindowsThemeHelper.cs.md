# `.\better-genshin-impact\Build\MicaSetup\Design\Themes\WindowsThemeHelper.cs`

```cs
# 引入 Microsoft.Win32 命名空间
﻿using Microsoft.Win32;

# 定义命名空间
namespace MicaSetup.Design.Controls;

# 定义静态类 WindowsThemeHelper
public static class WindowsThemeHelper
{
    # 定义获取当前 Windows 主题的静态方法
    public static WindowsTheme GetCurrentWindowsTheme()
    {
        # 打开注册表中的指定子键
        using RegistryKey key = Registry.CurrentUser.OpenSubKey(@"Software\Microsoft\Windows\CurrentVersion\Themes\Personalize");
        # 获取指定注册表值
        object? registryValueObject = key?.GetValue("AppsUseLightTheme");

        # 如果注册表值为 null，则返回 Light 主题
        if (registryValueObject == null)
        {
            return WindowsTheme.Light;
        }

        # 将注册表值转换为整数
        var registryValue = (int)registryValueObject;

        # 根据注册表值返回相应的主题，值大于 0 返回 Light 主题，否则返回 Dark 主题
        return registryValue > 0 ? WindowsTheme.Light : WindowsTheme.Dark;
    }
}
```