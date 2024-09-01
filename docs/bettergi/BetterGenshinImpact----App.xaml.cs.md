# `.\better-genshin-impact\BetterGenshinImpact\App.xaml.cs`

```cs
﻿using BetterGenshinImpact.GameTask; // 引入游戏任务相关的命名空间
using BetterGenshinImpact.Helpers; // 引入帮助程序相关的命名空间
using BetterGenshinImpact.Helpers.Extensions; // 引入扩展方法的命名空间
using BetterGenshinImpact.Service; // 引入服务相关的命名空间
using BetterGenshinImpact.Service.Interface; // 引入服务接口相关的命名空间
using BetterGenshinImpact.Service.Notification; // 引入通知服务相关的命名空间
using BetterGenshinImpact.Service.Notifier; // 引入通知器相关的命名空间
using BetterGenshinImpact.View; // 引入视图相关的命名空间
using BetterGenshinImpact.View.Pages; // 引入视图页面相关的命名空间
using BetterGenshinImpact.ViewModel; // 引入视图模型相关的命名空间
using BetterGenshinImpact.ViewModel.Pages; // 引入视图模型页面相关的命名空间
using Microsoft.Extensions.DependencyInjection; // 引入 Microsoft 扩展依赖注入相关的命名空间
using Microsoft.Extensions.Hosting; // 引入 Microsoft 扩展主机相关的命名空间
using Microsoft.Extensions.Logging; // 引入 Microsoft 扩展日志记录相关的命名空间
using Serilog; // 引入 Serilog 日志记录库
using Serilog.Events; // 引入 Serilog 事件相关的命名空间
using Serilog.Sinks.RichTextBox.Abstraction; // 引入 Serilog 富文本框抽象的命名空间
using System; // 引入系统基础类库
using System.Diagnostics; // 引入系统调试功能相关的命名空间
using System.IO; // 引入系统文件输入输出功能相关的命名空间
using System.Threading.Tasks; // 引入异步任务相关的命名空间
using System.Windows; // 引入 WPF 窗口功能相关的命名空间
using Wpf.Ui; // 引入 WPF 用户界面相关的命名空间

namespace BetterGenshinImpact; // 定义命名空间 BetterGenshinImpact

public partial class App : Application // 定义部分应用程序类，继承自 WPF 的 Application 类
{
    // The.NET Generic Host provides dependency injection, configuration, logging, and other services.
    // https://docs.microsoft.com/dotnet/core/extensions/generic-host
    // https://docs.microsoft.com/dotnet/core/extensions/dependency-injection
    // https://docs.microsoft.com/dotnet/core/extensions/configuration
    // https://docs.microsoft.com/dotnet/core/extensions/logging
    public static ILogger<T> GetLogger<T>() // 静态方法，获取指定类型的日志记录器
    {
        return _host.Services.GetService<ILogger<T>>()!; // 从服务提供者获取 ILogger<T> 服务
    }

    /// <summary>
    /// Gets registered service.
    /// </summary>
    /// <typeparam name="T">Type of the service to get.</typeparam>
    /// <returns>Instance of the service or <see langword="null"/>.</returns>
    public static T? GetService<T>() where T : class // 泛型静态方法，根据类型获取服务实例
    {
        return _host.Services.GetService(typeof(T)) as T; // 从服务提供者获取服务并转换为指定类型
    }

    /// <summary>
    /// Gets registered service.
    /// </summary>
    /// <returns>Instance of the service or <see langword="null"/>.</returns>
    /// <returns></returns>
    public static object? GetService(Type type) // 静态方法，根据类型获取服务实例
    {
        return _host.Services.GetService(type); // 从服务提供者获取服务
    }

    /// <summary>
    /// Occurs when the application is loading.
    /// </summary>
    protected override async void OnStartup(StartupEventArgs e) // 重写 OnStartup 方法，当应用程序启动时调用
    {
        base.OnStartup(e); // 调用基类的 OnStartup 方法

        try
        {
            RegisterEvents(); // 注册事件
            await _host.StartAsync(); // 异步启动主机
            await UrlProtocolHelper.RegisterAsync(); // 异步注册 URL 协议
        }
        catch (Exception ex) // 捕获异常
        {
            // DEBUG only, no overhead
            Debug.WriteLine(ex); // 将异常信息输出到调试器

            if (Debugger.IsAttached) // 检查调试器是否附加
            {
                Debugger.Break(); // 触发调试断点
            }
        }
    }

    /// <summary>
    /// Occurs when the application is closing.
    /// </summary>
    protected override async void OnExit(ExitEventArgs e) // 重写 OnExit 方法，当应用程序退出时调用
    {
        base.OnExit(e); // 调用基类的 OnExit 方法

        await _host.StopAsync(); // 异步停止主机
        _host.Dispose(); // 释放主机资源
    }

    /// <summary>
    /// 注册事件
    /// </summary>
    private void RegisterEvents() // 私有方法，用于注册事件
    {
        // 订阅 TaskScheduler 的未捕获异常事件处理
        TaskScheduler.UnobservedTaskException += TaskSchedulerUnobservedTaskException;

        // 订阅 UI 线程的未捕获异常事件处理
        this.DispatcherUnhandledException += AppDispatcherUnhandledException;

        // 订阅非 UI 线程的未捕获异常事件处理
        AppDomain.CurrentDomain.UnhandledException += CurrentDomainUnhandledException;
    }

    // 处理 Task 线程中的未捕获异常
    private static void TaskSchedulerUnobservedTaskException(object? sender, UnobservedTaskExceptionEventArgs e)
    {
        try
        {
            // 处理异常
            HandleException(e.Exception);
        }
        catch (Exception ex)
        {
            // 捕获处理异常中的异常
            HandleException(ex);
        }
        finally
        {
            // 标记异常已被观察
            e.SetObserved();
        }
    }

    // 处理非 UI 线程中的未捕获异常
    private static void CurrentDomainUnhandledException(object sender, UnhandledExceptionEventArgs e)
    {
        try
        {
            // 如果异常对象是 Exception 类型，处理异常
            if (e.ExceptionObject is Exception exception)
            {
                HandleException(exception);
            }
        }
        catch (Exception ex)
        {
            // 捕获处理异常中的异常
            HandleException(ex);
        }
        finally
        {
            // 忽略异常
        }
    }

    // 处理 UI 线程中的未捕获异常
    private static void AppDispatcherUnhandledException(object sender, System.Windows.Threading.DispatcherUnhandledExceptionEventArgs e)
    {
        try
        {
            // 处理异常
            HandleException(e.Exception);
        }
        catch (Exception ex)
        {
            // 捕获处理异常中的异常
            HandleException(ex);
        }
        finally
        {
            // 标记异常已处理
            e.Handled = true;
        }
    }

    // 处理异常并显示错误信息
    private static void HandleException(Exception e)
    {
        if (e.InnerException != null)
        {
            // 如果存在内部异常，使用内部异常
            e = e.InnerException;
        }

        // 显示错误信息
        MessageBox.Error("程序异常：" + e.Source + "\r\n--" + Environment.NewLine + e.StackTrace + "\r\n---" + Environment.NewLine + e.Message);

        // 记录日志
        GetLogger<App>().LogDebug(e, "UnHandle Exception");
    }
你给的代码示例似乎不完整。能否提供更多的代码上下文或是完整的代码片段？这样我能更好地帮助你添加注释。
```