# `.\better-genshin-impact\BetterGenshinImpact\Service\ApplicationHostService.cs`

```cs
# 引入所需的命名空间
﻿using BetterGenshinImpact.View;
using BetterGenshinImpact.View.Pages;
using Microsoft.Extensions.Hosting;
using System;
using System.Linq;
using System.Threading;
using System.Threading.Tasks;
using System.Windows;
using Wpf.Ui;

namespace BetterGenshinImpact.Service;

/// <summary>
/// 应用程序的托管服务类.
/// </summary>
public class ApplicationHostService : IHostedService
{
    # 服务提供者，用于依赖注入
    private readonly IServiceProvider _serviceProvider;
    # 导航窗口的实例
    private INavigationWindow? _navigationWindow;

    # 构造函数，接受服务提供者作为参数
    public ApplicationHostService(IServiceProvider serviceProvider)
    {
        _serviceProvider = serviceProvider;
    }

    /// <summary>
    /// 当应用程序主机准备启动服务时触发.
    /// </summary>
    /// <param name="cancellationToken">表示启动过程已被中止.</param>
    public async Task StartAsync(CancellationToken cancellationToken)
    {
        # 调用处理激活的方法
        await HandleActivationAsync();
    }

    /// <summary>
    /// 当应用程序主机正在进行正常关闭时触发.
    /// </summary>
    /// <param name="cancellationToken">表示关闭过程不再正常.</param>
    public async Task StopAsync(CancellationToken cancellationToken)
    {
        # 异步完成任务，不执行实际操作
        await Task.CompletedTask;
    }

    /// <summary>
    /// 在激活过程中创建主窗口.
    /// </summary>
    private async Task HandleActivationAsync()
    {
        # 异步完成任务，不执行实际操作
        await Task.CompletedTask;

        # 如果当前没有主窗口，创建并显示导航窗口
        if (!Application.Current.Windows.OfType<MainWindow>().Any())
        {
            _navigationWindow = (
                _serviceProvider.GetService(typeof(INavigationWindow)) as INavigationWindow
            )!;
            # 显示导航窗口
            _navigationWindow!.ShowWindow();

            # 导航到主页
            _navigationWindow.Navigate(typeof(HomePage));
        }

        # 异步完成任务，不执行实际操作
        await Task.CompletedTask;
    }
}
```