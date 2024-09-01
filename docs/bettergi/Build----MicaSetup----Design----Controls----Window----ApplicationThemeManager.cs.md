# `.\better-genshin-impact\Build\MicaSetup\Design\Controls\Window\ApplicationThemeManager.cs`

```cs
# 引入 MicaSetup.Natives 命名空间
﻿using MicaSetup.Natives;
# 引入 Microsoft.Win32 命名空间
using Microsoft.Win32;

# 定义命名空间 MicaSetup.Design.Controls
namespace MicaSetup.Design.Controls;

# 定义 ApplicationThemeManager 类
public class ApplicationThemeManager
{
    # 定义获取应用主题的静态方法
    public static ApplicationTheme GetAppTheme()
    {
        # 打开注册表项以获取用户主题设置
        using RegistryKey? key = Registry.CurrentUser.OpenSubKey(@"Software\Microsoft\Windows\CurrentVersion\Themes\Personalize");
        # 读取注册表中的 "AppsUseLightTheme" 值
        object? registryValueObject = key?.GetValue("AppsUseLightTheme");

        # 如果注册表值为 null，返回 Light 主题
        if (registryValueObject == null)
        {
            return ApplicationTheme.Light;
        }

        # 将注册表值转换为整数
        var registryValue = (int)registryValueObject;

        # 根据注册表值返回 Light 主题或 Dark 主题
        return registryValue > 0 ? ApplicationTheme.Light : ApplicationTheme.Dark;
    }
}
```