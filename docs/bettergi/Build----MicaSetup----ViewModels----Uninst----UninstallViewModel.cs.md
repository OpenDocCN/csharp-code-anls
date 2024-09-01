# `.\better-genshin-impact\Build\MicaSetup\ViewModels\Uninst\UninstallViewModel.cs`

```cs
﻿using CommunityToolkit.Mvvm.ComponentModel;  // 引入 CommunityToolkit.Mvvm.ComponentModel 命名空间，用于 MVVM 的支持
using MicaSetup.Design.Controls;  // 引入 MicaSetup.Design.Controls 命名空间，包含 UI 控件
using MicaSetup.Helper;  // 引入 MicaSetup.Helper 命名空间，包含辅助工具类
using MicaSetup.Services;  // 引入 MicaSetup.Services 命名空间，包含服务相关的类
using System;  // 引入 System 命名空间，包含基本的系统功能
using System.IO;  // 引入 System.IO 命名空间，提供文件和流的操作功能
using System.Threading.Tasks;  // 引入 System.Threading.Tasks 命名空间，支持异步编程

namespace MicaSetup.ViewModels  // 定义 MicaSetup.ViewModels 命名空间
{
    public partial class UninstallViewModel : ObservableObject  // 定义 UninstallViewModel 类，继承自 ObservableObject（支持属性更改通知）
    {
        [ObservableProperty]  // 使用 ObservableProperty 特性，自动实现属性的通知功能
        private string installInfo = string.Empty;  // 定义 installInfo 字段，表示安装信息，初始值为空字符串

        [ObservableProperty]  // 使用 ObservableProperty 特性，自动实现属性的通知功能
        private double installProgress = 0d;  // 定义 installProgress 字段，表示安装进度，初始值为 0

        public UninstallViewModel()  // UninstallViewModel 构造函数
        {
            Option.Current.Uninstalling = true;  // 设置当前选项的 Uninstalling 属性为 true，表示正在卸载

            InstallInfo = Mui("Preparing");  // 设置 InstallInfo 属性的值为 "Preparing" 的本地化字符串

            Task.Run(async () =>  // 启动一个新的异步任务
            {
                await Task.Delay(200);  // 延迟 200 毫秒

                InstallInfo = Mui("ProgressTipsUninstalling");  // 更新 InstallInfo 属性的值为 "ProgressTipsUninstalling" 的本地化字符串

                UninstallHelper.Uninstall((progress, key) =>  // 调用 UninstallHelper 的 Uninstall 方法，传入进度回调和报告回调
                {
                    UIDispatcherHelper.BeginInvoke(() =>  // 使用 UIDispatcherHelper 在 UI 线程上执行操作
                    {
                        InstallProgress = progress * 100d;  // 更新 InstallProgress 属性的值为进度的百分比
                        InstallInfo = key;  // 更新 InstallInfo 属性的值为回调中的键
                    });
                }, (report, _) =>  // 传入报告回调
                {
                    if (report == UninstallReport.AnyDeleteDelayUntilReboot)  // 如果报告中表示需要重启
                    {
                        UIDispatcherHelper.Invoke(main =>  // 使用 UIDispatcherHelper 在 UI 线程上执行操作
                        {
                            _ = MessageBoxX.Info(main, Mui("UninstallDelayUntilRebootTips"));  // 显示信息框提示用户重启
                        });
                    }
                });

                if (Option.Current.IsAllowFirewall)  // 如果选项中允许防火墙操作
                {
                    try
                    {
                        FirewallHelper.RemoveApplication(Path.Combine(Option.Current.InstallLocation ?? string.Empty, Option.Current.ExeName));  // 尝试从防火墙中移除应用程序
                    }
                    catch (Exception e)  // 捕获异常
                    {
                        Logger.Error(e);  // 记录错误日志
                    }
                }

                Option.Current.Uninstalling = false;  // 设置当前选项的 Uninstalling 属性为 false，表示卸载完成
                await Task.Delay(200);  // 延迟 200 毫秒

                if (Option.Current.IsRefreshExplorer)  // 如果选项中需要刷新资源管理器
                {
                    try
                    {
                        ServiceManager.GetService<IExplorerService>().Refresh();  // 尝试刷新资源管理器
                    }
                    catch (Exception e)  // 捕获异常
                    {
                        Logger.Error(e);  // 记录错误日志
                    }
                }

                UIDispatcherHelper.Invoke(Routing.GoToNext);  // 使用 UIDispatcherHelper 在 UI 线程上执行路由到下一个操作
            }).Forget();  // 忽略任务的返回结果
        }
    }
}
```