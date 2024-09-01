# `.\better-genshin-impact\Build\MicaSetup\Design\ThemeResourceDictionary.cs`

```cs
# 引入 MicaSetup.Design.Controls 命名空间中的控件
﻿using MicaSetup.Design.Controls;
# 引入系统命名空间
using System;
# 引入 Windows 命名空间
using System.Windows;

# 定义一个密封类 ThemeResourceDictionary，继承自 ResourceDictionary
namespace MicaSetup.Design;

public sealed class ThemeResourceDictionary : ResourceDictionary
{
    # 定义一个静态属性 Instance，用于获取和设置 ThemeResourceDictionary 实例，初始值为 null!
    public static ThemeResourceDictionary Instance { get; private set; } = null!;

    # 构造函数 ThemeResourceDictionary
    public ThemeResourceDictionary()
    {
        # 将当前实例赋值给静态属性 Instance
        Instance = this;
        # 将暗黑主题的 ResourceDictionary 添加到合并的字典中
        MergedDictionaries.Add(new ResourceDictionary()
        {
            Source = new Uri($"pack://application:,,,/MicaSetup;component/Resources/Themes/Dark.xaml"),
        });
        # 将亮色主题的 ResourceDictionary 添加到合并的字典中
        MergedDictionaries.Add(new ResourceDictionary()
        {
            Source = new Uri($"pack://application:,,,/MicaSetup;component/Resources/Themes/Light.xaml"),
        });
        # 将画刷资源的 ResourceDictionary 添加到合并的字典中
        MergedDictionaries.Add(new ResourceDictionary()
        {
            Source = new Uri($"pack://application:,,,/MicaSetup;component/Resources/Themes/Brushes.xaml"),
        });
    }

    # 定义一个静态方法 SyncResource，用于同步主题资源
    public static void SyncResource()
    {
        # 清除合并字典中的所有项
        Instance.MergedDictionaries.Clear();
        # 判断当前主题是否为暗黑主题
        if (ThemeService.Current.CurrentTheme == WindowsTheme.Dark)
        {
            # 如果是暗黑主题，添加暗黑主题的 ResourceDictionary 到合并字典中
            Instance.MergedDictionaries.Add(new ResourceDictionary()
            {
                Source = new Uri($"pack://application:,,,/MicaSetup;component/Resources/Themes/Dark.xaml"),
            });
            # 添加画刷资源的 ResourceDictionary 到合并字典中
            Instance.MergedDictionaries.Add(new ResourceDictionary()
            {
                Source = new Uri($"pack://application:,,,/MicaSetup;component/Resources/Themes/Brushes.xaml"),
            });
        }
        else
        {
            # 如果不是暗黑主题，添加亮色主题的 ResourceDictionary 到合并字典中
            Instance.MergedDictionaries.Add(new ResourceDictionary()
            {
                Source = new Uri($"pack://application:,,,/MicaSetup;component/Resources/Themes/Light.xaml"),
            });
            # 添加画刷资源的 ResourceDictionary 到合并字典中
            Instance.MergedDictionaries.Add(new ResourceDictionary()
            {
                Source = new Uri($"pack://application:,,,/MicaSetup;component/Resources/Themes/Brushes.xaml"),
            });
        }
    }
}
```