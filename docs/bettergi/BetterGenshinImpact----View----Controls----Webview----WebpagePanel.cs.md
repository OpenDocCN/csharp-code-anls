# `.\better-genshin-impact\BetterGenshinImpact\View\Controls\Webview\WebpagePanel.cs`

```cs
# 引入 WebView2 控件所需的命名空间
﻿using Microsoft.Web.WebView2.Core;
using Microsoft.Web.WebView2.Wpf;
using System;
using System.Diagnostics;
using System.IO;
using System.Text;
using System.Threading;
using System.Windows;
using System.Windows.Controls;

namespace BetterGenshinImpact.View.Controls.Webview;

# 定义 WebpagePanel 类，继承自 UserControl
public class WebpagePanel : UserControl
{
    # 当前 URI 的私有字段
    private Uri _currentUri = null!;
    # WebView2 控件的私有字段
    private WebView2 _webView = null!;
    # 表示是否已初始化的私有字段
    private bool _initialized = false;

    # 构造函数，初始化 WebpagePanel
    public WebpagePanel()
    {
        # 检查 WebView2 是否可用，如果不可用则显示下载按钮
        if (!IsWebView2Available())
        {
            Content = CreateDownloadButton();
        }
        else
        {
            # 创建 WebView2 实例并设置其属性
            _webView = new WebView2()
            {
                CreationProperties = new CoreWebView2CreationProperties
                {
                    UserDataFolder = Path.Combine(new FileInfo(Environment.ProcessPath!).DirectoryName!, @"WebView2Data\\"),
                    AdditionalBrowserArguments = "--enable-features=WebContentsForceDark"
                }
            };
            # 订阅 WebView2 初始化完成事件
            _webView.CoreWebView2InitializationCompleted += WebView_CoreWebView2InitializationCompleted;
            # 订阅 WebView2 导航开始事件
            _webView.NavigationStarting += NavigationStarting_CancelNavigation;
            # 将 WebView2 控件设置为当前控件的内容
            Content = _webView;
        }
    }

    # WebView2 初始化完成事件处理方法
    private void WebView_CoreWebView2InitializationCompleted(object? sender, CoreWebView2InitializationCompletedEventArgs e)
    {
        # 如果初始化成功，将 _initialized 标记为 true
        if (e.IsSuccess)
        {
            _initialized = true;
        }
        else
        {
            # 忽略初始化异常
            _ = e.InitializationException;
        }
    }

    # 静态方法，检查 WebView2 是否可用
    public static bool IsWebView2Available()
    {
        try
        {
            # 尝试获取可用的浏览器版本字符串，如果成功则返回 true
            return !string.IsNullOrEmpty(CoreWebView2Environment.GetAvailableBrowserVersionString());
        }
        catch (Exception)
        {
            # 捕获异常，返回 false
            return false;
        }
    }

    # 静态方法，将文件路径转换为文件 URL
    public static Uri FilePathToFileUrl(string filePath)
    {
        var uri = new StringBuilder();
        # 遍历文件路径的每个字符，并构建 URL
        foreach (var v in filePath)
            if (v >= 'a' && v <= 'z' || v >= 'A' && v <= 'Z' || v >= '0' && v <= '9' ||
                v == '+' || v == '/' || v == ':' || v == '.' || v == '-' || v == '_' || v == '~' ||
                v > '\x80')
                uri.Append(v);
            else if (v == Path.DirectorySeparatorChar || v == Path.AltDirectorySeparatorChar)
                uri.Append('/');
            else
                uri.Append($"%{(int)v:X2}");
        # 如果路径以 UNC 路径开头，添加 "file:" 前缀，否则添加 "file:///" 前缀
        if (uri.Length >= 2 && uri[0] == '/' && uri[1] == '/') // UNC path
            uri.Insert(0, "file:");
        else
            uri.Insert(0, "file:///");

        try
        {
            # 尝试创建并返回 URI 对象
            return new Uri(uri.ToString());
        }
        catch
        {
            # 捕获异常并返回空 URI
            return null!;
        }
    }

    # 导航到指定文件的方法
    public void NavigateToFile(string path)
    {
        # 如果路径是绝对路径，则将其转换为文件 URL，否则直接创建 URI 对象
        var uri = Path.IsPathRooted(path) ? FilePathToFileUrl(path) : new Uri(path);

        # 导航到指定 URI
        NavigateToUri(uri);
    }

    # 导航到指定 URI 的方法
    public void NavigateToUri(Uri uri)
    {
        # 如果 WebView2 控件为空，则退出方法
        if (_webView == null)
            return;

        # 设置 WebView2 的源 URI，并更新当前 URI
        _webView.Source = uri;
        _currentUri = _webView.Source;
    }
}
    // 导航到指定的 HTML 内容
    public void NavigateToHtml(string html)
    {
        // 确保 WebView2 控件的核心组件已初始化，异步执行后续操作
        _webView?.EnsureCoreWebView2Async()
            // 等待直到 _initialized 为 true
            .ContinueWith(_ => SpinWait.SpinUntil(() => _initialized))
            // 在 UI 线程中导航到 HTML 字符串
            .ContinueWith(_ => Dispatcher.Invoke(() => _webView?.NavigateToString(html)));
    }

    // 处理导航开始事件，决定是否取消导航
    private void NavigationStarting_CancelNavigation(object? sender, CoreWebView2NavigationStartingEventArgs e)
    {
        // 如果导航 URI 是以 "data:" 开头，表示使用了 NavigateToString，直接返回不做处理
        if (e.Uri.StartsWith("data:")) // when using NavigateToString
            return;

        // 将 URI 转换为 Uri 对象
        var newUri = new Uri(e.Uri);
        // 如果新 URI 与当前 URI 不同，则取消导航
        if (newUri != _currentUri) e.Cancel = true;
    }

    // 释放资源
    public void Dispose()
    {
        // 释放 WebView 资源
        _webView?.Dispose();
        // 将 WebView 设为 null
        _webView = null!;
    }

    // 创建一个下载按钮
    private Button CreateDownloadButton()
    {
        // 创建一个新的按钮并设置其属性
        var button = new Button
        {
            // 按钮内容
            Content = "查看需要安装 Microsoft Edge WebView2 点击这里开始下载",
            // 水平对齐方式
            HorizontalAlignment = HorizontalAlignment.Center,
            // 垂直对齐方式
            VerticalAlignment = VerticalAlignment.Center,
            // 按钮内边距
            Padding = new Thickness(20, 6, 20, 6)
        };
        // 为按钮点击事件添加处理程序，启动下载链接
        button.Click += (sender, e) => Process.Start("https://go.microsoft.com/fwlink/p/?LinkId=2124703");

        // 返回创建的按钮
        return button;
    }
这个代码片段仅包含一个右花括号 `}`，在实际编程中，它可能是某个代码块的结束标志。如果你有具体的代码段需要注释，请提供详细代码。
```