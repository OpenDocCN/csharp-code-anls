# `.\better-genshin-impact\Build\MicaSetup\Design\Themes\ThemeService.cs`

```cs
# 引入所需的命名空间
﻿using MicaSetup.Helper;
using MicaSetup.Natives;
using System;
using System.Windows;
using System.Windows.Interop;
using System.Windows.Media;

namespace MicaSetup.Design.Controls;

# 定义一个名为 ThemeService 的类
public class ThemeService
{
    # 定义一个公共的静态属性 Current，提供 ThemeService 的实例
    public static ThemeService Current { get; } = new();

    # 定义一个私有字段，存储当前的 Windows 主题
    private WindowsTheme currentTheme = WindowsTheme.Auto;

    # 定义一个公共属性 CurrentTheme，用于获取或设置当前的 Windows 主题
    public WindowsTheme CurrentTheme
    {
        # 获取当前主题
        get => GetTheme();
        # 设置当前主题并同步资源
        private set
        {
            currentTheme = value;
            ThemeResourceDictionary.SyncResource();
        }
    }

    # 定义一个公共方法 EnableBackdrop，用于为指定窗口启用背景效果
    public void EnableBackdrop(Window window, BackdropType micaType = BackdropType.Mica)
    {
        SetWindowBackdrop(window, micaType);
    }

    # 定义一个私有方法 SetWindowBackdrop，用于设置窗口的背景效果
    private void SetWindowBackdrop(Window window, BackdropType micaType)
    {
        # 如果操作系统版本低于 Windows 11，则退出方法
        if (!OsVersionHelper.IsWindows11_OrGreater)
        {
            return;
        }

        # 设置窗口背景颜色为透明白色
        window.Background = new SolidColorBrush(Color.FromArgb(0, 255, 255, 255));
        # 获取窗口的句柄
        var windowHandle = new WindowInteropHelper(window).Handle;

        # 根据当前主题设置窗口的暗色模式属性
        if (CurrentTheme == WindowsTheme.Dark)
        {
            NativeMethods.SetWindowAttribute(windowHandle, DWMWINDOWATTRIBUTE.DWMWA_USE_IMMERSIVE_DARK_MODE, 1);
        }
        else
        {
            NativeMethods.SetWindowAttribute(windowHandle, DWMWINDOWATTRIBUTE.DWMWA_USE_IMMERSIVE_DARK_MODE, 0);
        }

        # 根据 Windows 版本设置窗口的背景效果属性
        if (OsVersionHelper.IsWindows11_22523_OrGreater)
        {
            NativeMethods.SetWindowAttribute(windowHandle, DWMWINDOWATTRIBUTE.DWMWA_SYSTEMBACKDROP_TYPE, (int)micaType);
        }
        else
        {
            NativeMethods.SetWindowAttribute(windowHandle, DWMWINDOWATTRIBUTE.DWMWA_MICA_EFFECT, 1);
        }
    }

    # 定义一个私有方法 GetTheme，用于获取当前主题
    private WindowsTheme GetTheme()
    {
        # 如果当前主题是自动，则获取系统当前主题，否则返回设置的主题
        return currentTheme == WindowsTheme.Auto ? WindowsThemeHelper.GetCurrentWindowsTheme() : currentTheme;
    }

    # 定义一个公共方法 SetTheme，用于设置主题并同步主题资源
    public void SetTheme(WindowsTheme theme)
    {
        CurrentTheme = theme;
        SyncThemeResource();
    }

    # 定义一个私有方法 SyncThemeResource，意图同步主题资源（代码未完整）
    private bool SyncThemeResource()
    {
        # 检查当前应用程序是否为空
        if (Application.Current == null)
        {
            # 如果应用程序为空，返回 false
            return false;
        }

        # 获取当前主题的名称，并转换为字符串
        string name = GetTheme().ToString();

        try
        {
            # 遍历应用程序资源中的所有已合并的资源字典
            foreach (ResourceDictionary dictionary in Application.Current.Resources.MergedDictionaries)
            {
                # 检查当前资源字典是否是 ThemeResourceDictionary 类型
                if (dictionary is ThemeResourceDictionary trd)
                {
                    # 遍历主题资源字典中的所有已合并的资源字典
                    foreach (ResourceDictionary td in trd.MergedDictionaries)
                    {
                        # 检查字典的 Source 属性是否不为空，并且其原始字符串是否与目标主题文件路径匹配
                        if (dictionary.Source != null && dictionary.Source.OriginalString.Equals($"/Resources/Themes/{name}.xaml", StringComparison.Ordinal))
                        {
                            # 从应用程序资源的已合并字典中移除匹配的字典
                            Application.Current.Resources.MergedDictionaries.Remove(dictionary);
                            # 将匹配的字典重新添加到已合并的字典中
                            Application.Current.Resources.MergedDictionaries.Add(dictionary);
                            # 返回 true 表示操作成功
                            return true;
                        }
                    }
                }
            }
        }
        catch (Exception e)
        {
            # 捕捉并忽略任何异常
            _ = e;
        }
        # 如果没有找到匹配的字典或发生异常，返回 false
        return false;
    }
# 结束当前的代码块或代码结构
}
```