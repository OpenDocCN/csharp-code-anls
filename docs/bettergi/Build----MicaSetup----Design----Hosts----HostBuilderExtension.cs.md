# `.\better-genshin-impact\Build\MicaSetup\Design\Hosts\HostBuilderExtension.cs`

```cs
# 导入所需的命名空间
﻿using MicaSetup.Helper;  # MicaSetup.Helper 命名空间
using MicaSetup.Natives;  # MicaSetup.Natives 命名空间
using MicaSetup.Services;  # MicaSetup.Services 命名空间
using Microsoft.Extensions.DependencyInjection;  # Microsoft.Extensions.DependencyInjection 命名空间
using System;  # System 命名空间
using System.Collections.Generic;  # System.Collections.Generic 命名空间
using System.Globalization;  # System.Globalization 命名空间
using System.Threading;  # System.Threading 命名空间
using System.Threading.Tasks;  # System.Threading.Tasks 命名空间
using System.Windows;  # System.Windows 命名空间
using System.Windows.Threading;  # System.Windows.Threading 命名空间

namespace MicaSetup.Design.Controls;  # 定义命名空间 MicaSetup.Design.Controls

#pragma warning disable IDE0002  # 禁用 IDE0002 警告（代码简化建议）

public static class HostBuilderExtension  # 定义静态类 HostBuilderExtension
{
    public static IHostBuilder UseHostBuilder(this IHostBuilder builder, Action<IHostBuilder> custom)  # 扩展 IHostBuilder，接受自定义操作
    {
        custom?.Invoke(builder);  # 如果 custom 不为空，则调用自定义操作
        return builder;  # 返回 IHostBuilder 实例
    }

    public static IHostBuilder UseAsUninst(this IHostBuilder builder)  # 扩展 IHostBuilder，将当前状态标记为卸载
    {
        Option.Current.IsUninst = true;  # 设置选项为卸载模式
        return builder;  # 返回 IHostBuilder 实例
    }

    public static IHostBuilder UseLogger(this IHostBuilder builder, bool enabled = true)  # 扩展 IHostBuilder，设置日志记录器的启用状态
    {
        Option.Current.Logging = enabled;  # 设置选项中的日志记录状态
        Logger.Info("Setup run started ...");  # 记录启动日志
        return builder;  # 返回 IHostBuilder 实例
    }

    public static IHostBuilder UseServices(this IHostBuilder builder, Action<IServiceCollection> service)  # 扩展 IHostBuilder，配置服务集合
    {
        ServiceCollection serviceCollection = new();  # 创建新的服务集合实例
        service?.Invoke(serviceCollection);  # 如果 service 不为空，则调用配置操作
        ServiceManager.Services = builder.ServiceProvider = serviceCollection.BuildServiceProvider();  # 构建服务提供者并赋值
        return builder;  # 返回 IHostBuilder 实例
    }

    public static IHostBuilder UseTempPathFork(this IHostBuilder builder)  # 扩展 IHostBuilder，处理临时路径分叉
    {
        if (RuntimeHelper.IsDebuggerAttached)  # 如果调试器附加，则返回原对象
        {
            return builder;  # 返回 IHostBuilder 实例
        }
        TempPathForkHelper.Fork();  # 调用临时路径分叉助手
        return builder;  # 返回 IHostBuilder 实例
    }

    public static IHostBuilder UseElevated(this IHostBuilder builder)  # 扩展 IHostBuilder，确保以提升权限运行
    {
        RuntimeHelper.EnsureElevated();  # 确保应用程序以提升权限运行
        return builder;  # 返回 IHostBuilder 实例
    }

    public static IHostBuilder UseDpiAware(this IHostBuilder builder)  # 扩展 IHostBuilder，设置 DPI 感知
    {
        _ = DpiAwareHelper.SetProcessDpiAwareness();  # 设置进程 DPI 感知
        return builder;  # 返回 IHostBuilder 实例
    }

    public static IHostBuilder UseSingleInstance(this IHostBuilder builder, string instanceName, Action<bool> callback = null!)  # 扩展 IHostBuilder，检查单实例运行
    {
        RuntimeHelper.CheckSingleInstance(instanceName, callback);  # 检查是否为单实例并调用回调
        return builder;  # 返回 IHostBuilder 实例
    }

    public static IHostBuilder UseLanguage(this IHostBuilder builder, string name)  # 扩展 IHostBuilder，设置语言文化
    {
        Thread.CurrentThread.CurrentUICulture = Thread.CurrentThread.CurrentCulture = new CultureInfo(name);  # 设置当前线程的 UI 文化和文化
        return builder;  # 返回 IHostBuilder 实例
    }

    public static IHostBuilder UseMuiLanguage(this IHostBuilder builder)  # 扩展 IHostBuilder，设置 MUI 语言
    {
        MuiLanguage.SetupLanguage();  # 配置 MUI 语言
        return builder;  # 返回 IHostBuilder 实例
    }

    public static IHostBuilder UseTheme(this IHostBuilder builder, WindowsTheme theme = WindowsTheme.Auto)  # 扩展 IHostBuilder，设置主题
    {
        ThemeService.Current.SetTheme(theme);  # 设置当前主题
        return builder;  # 返回 IHostBuilder 实例
    }

    public static IHostBuilder UseFonts(this IHostBuilder builder, Action<List<MuiLanguageFont>> handler)  # 扩展 IHostBuilder，配置字体
    {
        handler?.Invoke(MuiLanguage.FontSelector);  # 如果 handler 不为空，则调用配置操作
        return builder;  # 返回 IHostBuilder 实例
    }

    public static IHostBuilder UseOptions(this IHostBuilder builder, Action<Option> handler)  # 扩展 IHostBuilder，配置选项
    # 调用处理程序（如果存在）并传入当前选项
    handler?.Invoke(Option.Current);
    # 返回构建器对象
    return builder;

    # 扩展 IHostBuilder，使用提供的处理程序操作页面字典
    public static IHostBuilder UsePages(this IHostBuilder builder, Action<Dictionary<string, Type>> handler)
    {
        # 调用处理程序（如果存在）并传入页面字典
        handler?.Invoke(ShellPageSetting.PageDict);
        # 返回构建器对象
        return builder;
    }

    # 扩展 IHostBuilder，处理未捕获的 Dispatcher 异常
    public static IHostBuilder UseDispatcherUnhandledExceptionCatched(this IHostBuilder builder, DispatcherUnhandledExceptionEventHandler handler = null!)
    {
        # 如果构建器有应用程序
        if (builder?.App != null)
        {
            # 如果提供了异常处理程序
            if (handler != null)
            {
                # 如果应用程序存在，附加处理程序
                if (builder!.App is Application app)
                {
                    app.DispatcherUnhandledException += handler;
                }
            }
            # 如果没有提供处理程序
            else
            {
                # 如果应用程序存在，使用默认处理程序
                if (builder!.App is Application app)
                {
                    app.DispatcherUnhandledException += (object s, DispatcherUnhandledExceptionEventArgs e) =>
                    {
                        # 记录致命错误日志，并将异常标记为已处理
                        Logger.Fatal("Application.DispatcherUnhandledException", e?.Exception?.ToString()!);
                        e!.Handled = true;
                    };
                }
            }
        }
        # 返回构建器对象
        return builder!;
    }

    # 扩展 IHostBuilder，处理未捕获的 AppDomain 异常
    public static IHostBuilder UseDomainUnhandledExceptionCatched(this IHostBuilder builder, UnhandledExceptionEventHandler handler = null!)
    {
        # 如果提供了异常处理程序
        if (handler != null)
        {
            # 附加到 AppDomain 的未处理异常事件
            AppDomain.CurrentDomain.UnhandledException += handler;
        }
        # 如果没有提供处理程序
        else
        {
            # 使用默认处理程序记录异常
            AppDomain.CurrentDomain.UnhandledException += (object s, UnhandledExceptionEventArgs e) =>
            {
                Logger.Fatal("AppDomain.CurrentDomain.UnhandledException", e?.ExceptionObject?.ToString()!);
            };
        }
        # 返回构建器对象
        return builder;
    }

    # 扩展 IHostBuilder，处理未观察的任务异常
    public static IHostBuilder UseUnobservedTaskExceptionCatched(this IHostBuilder builder, EventHandler<UnobservedTaskExceptionEventArgs> handler = null!)
    {
        # 如果提供了异常处理程序
        if (handler != null)
        {
            # 附加到任务调度器的未观察异常事件
            TaskScheduler.UnobservedTaskException += handler;
        }
        # 如果没有提供处理程序
        else
        {
            # 使用默认处理程序记录异常并标记为已观察
            TaskScheduler.UnobservedTaskException += (s, e) =>
            {
                Logger.Fatal("TaskScheduler.UnobservedTaskException", e?.Exception?.ToString()!);
                e?.SetObserved();
            };
        }
        # 返回构建器对象
        return builder;
    }
请提供需要注释的代码片段。
```