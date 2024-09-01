# `.\better-genshin-impact\BetterGenshinImpact\ViewModel\MainWindowViewModel.cs`

```cs
# 引入 BetterGenshinImpact.Core.Config 命名空间，提供配置信息功能
﻿using BetterGenshinImpact.Core.Config;
# 引入 BetterGenshinImpact.Core.Recognition.OCR 命名空间，提供 OCR 功能
using BetterGenshinImpact.Core.Recognition.OCR;
# 引入 BetterGenshinImpact.Helpers 命名空间，提供辅助功能
using BetterGenshinImpact.Helpers;
# 引入 BetterGenshinImpact.Model 命名空间，提供数据模型
using BetterGenshinImpact.Model;
# 引入 BetterGenshinImpact.Service.Interface 命名空间，提供服务接口
using BetterGenshinImpact.Service.Interface;
# 引入 CommunityToolkit.Mvvm.ComponentModel 命名空间，提供 MVVM 组件模型功能
using CommunityToolkit.Mvvm.ComponentModel;
# 引入 CommunityToolkit.Mvvm.Input 命名空间，提供命令输入功能
using CommunityToolkit.Mvvm.Input;
# 引入 Microsoft.Extensions.Logging 命名空间，提供日志功能
using Microsoft.Extensions.Logging;
# 引入 OpenCvSharp 命名空间，提供 OpenCV 图像处理功能
using OpenCvSharp;
# 引入 System 命名空间，提供基础功能
using System;
# 引入 System.ComponentModel 命名空间，提供组件模型功能
using System.ComponentModel;
# 引入 System.Diagnostics 命名空间，提供调试功能
using System.Diagnostics;
# 引入 System.Diagnostics.CodeAnalysis 命名空间，提供代码分析功能
using System.Diagnostics.CodeAnalysis;
# 引入 System.Net.Http 命名空间，提供 HTTP 客户端功能
using System.Net.Http;
# 引入 System.Net.Http.Json 命名空间，提供 JSON 支持功能
using System.Net.Http.Json;
# 引入 System.Threading.Tasks 命名空间，提供异步编程功能
using System.Threading.Tasks;
# 引入 System.Windows 命名空间，提供 WPF 窗口功能
using System.Windows;
# 引入 Wpf.Ui 命名空间，提供 WPF 用户界面功能
using Wpf.Ui;

# 定义命名空间 BetterGenshinImpact.ViewModel，包含视图模型相关的类
namespace BetterGenshinImpact.ViewModel;

# 定义 MainWindowViewModel 类，继承自 ObservableObject 和 IViewModel 接口
public partial class MainWindowViewModel : ObservableObject, IViewModel
{
    # 声明一个只读的日志对象
    private readonly ILogger<MainWindowViewModel> _logger;
    # 声明一个只读的配置服务对象
    private readonly IConfigService _configService;
    # 定义窗口的标题，包含版本信息和调试标志
    public string Title => $"BetterGI · 更好的原神 · {Global.Version}{(RuntimeHelper.IsDebug ? " · Dev" : string.Empty)}";

    # 定义一个可观察属性，表示窗口是否可见
    [ObservableProperty]
    public bool _isVisible = true;

    # 定义一个可观察属性，表示窗口的状态
    [ObservableProperty]
    public WindowState _windowState = WindowState.Normal;

    # 定义一个配置对象
    public AllConfig Config { get; set; }

    # MainWindowViewModel 类的构造函数，注入导航服务和配置服务
    public MainWindowViewModel(INavigationService navigationService, IConfigService configService)
    {
        # 初始化配置服务对象
        _configService = configService;
        # 从配置服务中获取配置并赋值给 Config 属性
        Config = _configService.Get();
        # 获取当前视图模型的日志对象
        _logger = App.GetLogger<MainWindowViewModel>();
    }

    # 定义一个命令，执行隐藏窗口的操作
    [RelayCommand]
    private void OnHide()
    {
        # 设置窗口可见性为 false
        IsVisible = false;
    }

    # 定义一个命令，在窗口关闭时的处理逻辑
    [RelayCommand]
    private void OnClosing(CancelEventArgs e)
    {
        # 判断是否配置为退出到系统托盘
        if (Config.CommonConfig.ExitToTray)
        {
            # 如果是，则取消关闭事件
            e.Cancel = true;
            # 调用隐藏窗口的方法
            OnHide();
        }
    }

    # 定义一个异步命令，在窗口加载时的处理逻辑
    [RelayCommand]
    [SuppressMessage("CommunityToolkit.Mvvm.SourceGenerators.RelayCommandGenerator", "MVVMTK0039:Async void returning method annotated with RelayCommand")]
    private async void OnLoaded()
    {
        # 尝试执行以下代码块
        try
        {
            # 异步执行任务
            await Task.Run(() =>
            {
                # 尝试执行以下代码块
                try
                {
                    # 使用 PaddleOCR 处理图像，并将结果存储在变量 s 中
                    var s = OcrFactory.Paddle.Ocr(new Mat(Global.Absolute("Assets\\Model\\PaddleOCR\\test_ocr.png"), ImreadModes.Grayscale));
                    # 输出 PaddleOCR 处理结果到调试控制台
                    Debug.WriteLine("PaddleOcr预热结果:" + s);
                }
                # 捕捉并处理 PaddleOCR 处理过程中的异常
                catch (Exception e)
                {
                    # 输出异常信息到控制台
                    Console.WriteLine(e);
                    # 记录异常详细信息到日志
                    _logger.LogError("PaddleOcr预热异常：" + e.Source + "\r\n--" + Environment.NewLine + e.StackTrace + "\r\n---" + Environment.NewLine + e.Message);
                    var innerException = e.InnerException;
                    # 如果存在内部异常，记录其详细信息并抛出
                    if (innerException != null)
                    {
                        _logger.LogError("PaddleOcr预热内部异常：" + innerException.Source + "\r\n--" + Environment.NewLine + innerException.StackTrace + "\r\n---" + Environment.NewLine + innerException.Message);
                        throw innerException;
                    }
                    # 如果没有内部异常，则重新抛出原异常
                    else
                    {
                        throw;
                    }
                }
            });
        }
        # 捕捉并处理 Task.Run 任务中的异常
        catch (Exception e)
        {
            # 显示警告消息框，并显示异常信息
            MessageBox.Warning("PaddleOcr预热失败：" + e.Source + "\r\n--" + Environment.NewLine + e.StackTrace + "\r\n---" + Environment.NewLine + e.Message);
        }

        # 尝试执行以下代码块
        try
        {
            # 异步执行 GetNewestInfoAsync 方法
            await Task.Run(GetNewestInfoAsync);
        }
        # 捕捉并处理 GetNewestInfoAsync 执行中的异常
        catch (Exception e)
        {
            # 输出获取最新版本信息失败的消息到调试控制台
            Debug.WriteLine("获取最新版本信息失败：" + e.Source + "\r\n--" + Environment.NewLine + e.StackTrace + "\r\n---" + Environment.NewLine + e.Message);
            # 记录警告信息到日志
            _logger.LogWarning("获取 BetterGI 最新版本信息失败");
        }
    }

    # 声明 GetNewestInfoAsync 方法的异步任务
    private async Task GetNewestInfoAsync()
    {
        try
        {
            // 使用 HttpClient 实例发送 HTTP 请求
            using var httpClient = new HttpClient();
            // 从指定的 URL 异步获取 JSON 数据，并将其解析为 Notice 对象
            var notice = await httpClient.GetFromJsonAsync<Notice>(@"https://hui-config.oss-cn-hangzhou.aliyuncs.com/bgi/notice.json");
            // 检查 Notice 对象是否非空以及版本信息是否存在
            if (notice != null && !string.IsNullOrWhiteSpace(notice.Version))
            {
                // 如果当前版本是新版本
                if (Global.IsNewVersion(notice.Version))
                {
                    // 如果配置中存在不再显示新版本通知的结束版本，并且当前版本不在此范围内
                    if (!string.IsNullOrEmpty(Config.NotShowNewVersionNoticeEndVersion)
                        && !Global.IsNewVersion(Config.NotShowNewVersionNoticeEndVersion, notice.Version))
                    {
                        // 结束方法执行，不显示通知
                        return;
                    }

                    // 异步调用 UI 线程更新提示框
                    await UIDispatcherHelper.Invoke(async () =>
                    {
                        // 创建消息框并配置其属性
                        var uiMessageBox = new Wpf.Ui.Controls.MessageBox
                        {
                            Title = "更新提示", // 消息框标题
                            Content = $"存在最新版本 {notice.Version}，点击确定前往下载页面下载最新版本", // 消息框内容
                            PrimaryButtonText = "确定", // 主按钮文本
                            SecondaryButtonText = "不再提示", // 次按钮文本
                            CloseButtonText = "取消", // 关闭按钮文本
                            WindowStartupLocation = WindowStartupLocation.CenterOwner, // 窗口启动位置
                            Owner = Application.Current.MainWindow, // 消息框的拥有者窗口
                        };

                        // 显示消息框并获取用户的选择结果
                        var result = await uiMessageBox.ShowDialogAsync();
                        // 如果用户点击了主按钮
                        if (result == Wpf.Ui.Controls.MessageBoxResult.Primary)
                        {
                            // 启动浏览器打开下载页面
                            Process.Start(new ProcessStartInfo("https://bgi.huiyadan.com/download.html") { UseShellExecute = true });
                        }
                        // 如果用户点击了次按钮
                        else if (result == Wpf.Ui.Controls.MessageBoxResult.Secondary)
                        {
                            // 更新配置，标记为不再显示新版本通知
                            Config.NotShowNewVersionNoticeEndVersion = notice.Version;
                        }
                    });
                }
            }
        }
        catch (Exception e)
        {
            // 捕获异常并输出调试信息
            Debug.WriteLine("获取最新版本信息失败：" + e.Source + "\r\n--" + Environment.NewLine + e.StackTrace + "\r\n---" + Environment.NewLine + e.Message);
            // 记录警告日志，表示获取最新版本信息失败
            _logger.LogWarning("获取 BetterGI 最新版本信息失败");
        }
    }
# 代码块结束标志
}
```