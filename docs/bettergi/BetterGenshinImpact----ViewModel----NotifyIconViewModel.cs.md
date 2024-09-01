# `.\better-genshin-impact\BetterGenshinImpact\ViewModel\NotifyIconViewModel.cs`

```cs
# 引用 BetterGenshinImpact 命名空间下的辅助类和接口
using BetterGenshinImpact.Helpers;
using BetterGenshinImpact.Service.Interface;
using BetterGenshinImpact.View.Controls.Webview;
using CommunityToolkit.Mvvm.ComponentModel;
using CommunityToolkit.Mvvm.Input;
using Newtonsoft.Json;
using System;
using System.Collections.Generic;
using System.Net;
using System.Net.Http;
using System.Text;
using System.Threading.Tasks;
using System.Windows;
using System.Windows.Interop;
using Vanara.PInvoke;

namespace BetterGenshinImpact.ViewModel;

# NotifyIconViewModel 类继承自 ObservableObject，实现通知图标的视图模型
public partial class NotifyIconViewModel : ObservableObject
{
    # RelayCommand 特性标记的命令方法，显示或隐藏主窗口
    [RelayCommand]
    public void ShowOrHide()
    {
        # 如果主窗口可见，则隐藏它
        if (Application.Current.MainWindow.Visibility == Visibility.Visible)
        {
            Application.Current.MainWindow.Hide();
        }
        # 如果主窗口不可见，则激活、聚焦并显示它，同时显示任务栏图标
        else
        {
            Application.Current.MainWindow.Activate();
            Application.Current.MainWindow.Focus();
            Application.Current.MainWindow.Show();
            WindowBacktray.Show(Application.Current.MainWindow);
        }
    }

    # RelayCommand 特性标记的命令方法，退出应用程序
    [RelayCommand]
    public void Exit()
    {
        # 保存配置设置
        App.GetService<IConfigService>()?.Save();
        # 关闭应用程序
        Application.Current.Shutdown();
    }

    # RelayCommand 特性标记的命令方法，异步检查更新
    [RelayCommand]
    public async Task CheckUpdateAsync()
    {
        try
        {
            # 创建 HttpClient 实例用于发送 HTTP 请求
            using var httpClient = new HttpClient();
            # 设置 User-Agent 头
            httpClient.DefaultRequestHeaders.UserAgent.ParseAdd("Mozilla/5.0 (Windows NT 10.0; Win64; x64)");
            # 发送 GET 请求获取最新版本的 JSON 数据
            string jsonString = await httpClient.GetStringAsync(@"https://api.github.com/repos/babalae/better-genshin-impact/releases/latest");
            # 反序列化 JSON 数据为字典
            var jsonDict = JsonConvert.DeserializeObject<Dictionary<string, object>>(jsonString);

            # 如果 JSON 字典不为空
            if (jsonDict != null)
            {
                # 提取更新名称和说明
                string? name = jsonDict["name"] as string;
                string? body = jsonDict["body"] as string;
                # 格式化为 Markdown 文本
                string md = $"# {name}{new string('\n', 2)}{body}";

                # 将 Markdown 文本转义为 HTML 实体
                md = WebUtility.HtmlEncode(md);
                # 从资源文件中读取 Markdown 转 HTML 模板
                string md2html = ResourceHelper.GetString($"pack://application:,,,/Assets/Strings/md2html.html", Encoding.UTF8);
                # 替换模板中的内容占位符
                var html = md2html.Replace("{{content}}", md);

                # 创建 WebpageWindow 实例，设置窗口属性
                WebpageWindow win = new()
                {
                    Title = "更新日志",
                    Width = 800,
                    Height = 600,
                    Owner = Application.Current.MainWindow,
                    WindowStartupLocation = WindowStartupLocation.CenterOwner
                };

                # 导航到生成的 HTML 内容
                win.NavigateToHtml(html);
                # 显示窗口对话框
                win.ShowDialog();
            }
        }
        # 捕获并忽略任何异常
        catch (Exception e)
        {
            _ = e;
        }
    }
}

# 文件级静态类 WindowBacktray 用于处理窗口的最小化和隐藏
file static class WindowBacktray
{
    # 隐藏窗口并将其最小化
    public static void Hide(Window window)
    {
        if (window != null)
        {
            window.Visibility = Visibility.Hidden;
            window.WindowState = WindowState.Minimized;
        }
    }

    # 显示窗口（此方法未完整显示）
    public static void Show(Window window)
    {
        # 检查窗口对象是否不为 null
        if (window != null)
        {
            # 检查窗口的可见性是否不是可见
            if (window.Visibility != Visibility.Visible)
            {
                # 设置窗口为可见
                window.Visibility = Visibility.Visible;
            }
            # 检查窗口的状态是否为最小化
            if (window.WindowState == WindowState.Minimized)
            {
                # 获取当前主窗口的句柄
                nint hWnd = new WindowInteropHelper(Application.Current.MainWindow).Handle;
                # 通过句柄发送恢复窗口消息，将最小化的窗口恢复到正常状态
                _ = User32.SendMessage(hWnd, User32.WindowMessage.WM_SYSCOMMAND, User32.SysCommand.SC_RESTORE, IntPtr.Zero);
            }
        }
    }
请提供需要注释的具体代码片段。
```