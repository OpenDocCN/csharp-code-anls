# `.\better-genshin-impact\BetterGenshinImpact\View\Controls\Webview\WebpageWindow.cs`

```cs
# 引入必要的命名空间
﻿using System;
using System.Windows;
using System.Windows.Media;
using Wpf.Ui.Controls;

# 定义 WebpageWindow 类，继承自 Window
namespace BetterGenshinImpact.View.Controls.Webview;

public class WebpageWindow : Window
{
    # 属性 Webview，类型为 WebpagePanel，通过 Content 属性获取
    public WebpagePanel? Webview => Content as WebpagePanel;

    # 构造函数
    public WebpageWindow()
    {
        # 创建 WebpagePanel 实例并设置边距
        WebpagePanel wp = new()
        {
            Margin = new(8, 8, 0, 8)
        };

        # 将 WebpagePanel 设置为窗口的内容
        Content = wp;
        # 设置窗口的背景色为深灰色
        Background = new SolidColorBrush(Color.FromRgb(0x20, 0x20, 0x20));
    }

    # 重写 OnSourceInitialized 方法，用于初始化窗口源时的操作
    protected override void OnSourceInitialized(EventArgs e)
    {
        base.OnSourceInitialized(e);
        # 尝试应用系统背景
        TryApplySystemBackdrop();
    }

    # 尝试应用系统背景的私有方法
    private void TryApplySystemBackdrop()
    {
        # 如果 Mica 背景被支持，则应用 Mica 背景
        if (WindowBackdrop.IsSupported(WindowBackdropType.Mica))
        {
            WindowBackdrop.ApplyBackdrop(this, WindowBackdropType.Mica);
            return;
        }

        # 如果 Tabbed 背景被支持，则应用 Tabbed 背景
        if (WindowBackdrop.IsSupported(WindowBackdropType.Tabbed))
        {
            WindowBackdrop.ApplyBackdrop(this, WindowBackdropType.Tabbed);
            return;
        }
    }

    # 导航到指定的 URI
    public void NavigateToUri(Uri uri)
    {
        Webview?.NavigateToUri(uri);
    }

    # 导航到指定的 HTML 内容
    public void NavigateToHtml(string html)
    {
        Webview?.NavigateToHtml(html);
    }
}
```