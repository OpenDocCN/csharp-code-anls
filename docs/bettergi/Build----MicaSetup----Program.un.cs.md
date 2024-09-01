# `.\better-genshin-impact\Build\MicaSetup\Program.un.cs`

```cs
# 引入所需的命名空间
﻿using MicaSetup.Design.Controls;
using MicaSetup.Services;
using MicaSetup.Views;
using Microsoft.Extensions.DependencyInjection;
using System;
using System.Reflection;
using System.Runtime.InteropServices;

# 设置程序集的唯一标识符
[assembly: Guid("00000000-0000-0000-0000-000000000000")]
# 设置程序集的标题
[assembly: AssemblyTitle("BetterGI Uninst")]
# 设置程序集的产品名称
[assembly: AssemblyProduct("BetterGI")]
# 设置程序集的描述
[assembly: AssemblyDescription("BetterGI Uninst")]
# 设置程序集的公司名称
[assembly: AssemblyCompany("Lemutec")]
# 设置程序集的版权信息
[assembly: AssemblyCopyright("Under GPL-3.0 license. Copyright (c) better-genshin-impact Contributors.")]
# 设置程序集的版本信息
[assembly: AssemblyVersion("2.0.0.0")]
# 设置程序集的文件版本信息
[assembly: AssemblyFileVersion("2.0.0.0")]

# 定义命名空间
namespace MicaSetup;

# 定义内部类 Program
internal class Program
{
    # 标记为 STAThread，指定线程模型为单线程单元
    [STAThread]
    # 定义内部静态方法 Main，程序的入口点
    internal static void Main()
    {
        # 创建应用程序的构建器
        Hosting.CreateBuilder()
            # 配置为卸载程序
            .UseAsUninst()
            # 配置日志记录
            .UseLogger()
            # 配置为单实例应用程序
            .UseSingleInstance("BetterGI_MicaSetup")
            # 配置临时路径
            .UseTempPathFork()
            # 配置为以管理员权限运行
            .UseElevated()
            # 配置为 DPI 兼容
            .UseDpiAware()
            # 配置应用程序选项
            .UseOptions(option =>
            {
                # 创建桌面快捷方式
                option.IsCreateDesktopShortcut = true;
                # 创建卸载程序
                option.IsCreateUninst = true;
                # 创建注册表键
                option.IsCreateRegistryKeys = true;
                # 创建开始菜单快捷方式
                option.IsCreateStartMenu = true;
                # 不创建快速启动快捷方式
                option.IsCreateQuickLaunch = false;
                # 不设置为自动运行
                option.IsCreateAsAutoRun = false;
                # 使用注册表的 x86 版本（值为 null 表示不设置）
                option.IsUseRegistryPreferX86 = null!;
                # 允许防火墙
                option.IsAllowFirewall = true;
                # 刷新资源管理器
                option.IsRefreshExplorer = true;
                # 不安装证书
                option.IsInstallCertificate = false;
                # 可执行文件的路径
                option.ExeName = @"BetterGI\BetterGI.exe";
                # 注册表中的键名
                option.KeyName = "BetterGI";
                # 应用程序的显示名称
                option.DisplayName = "BetterGI";
                # 应用程序图标的路径
                option.DisplayIcon = @"BetterGI\BetterGI.exe";
                # 应用程序的显示版本
                option.DisplayVersion = "0.0.0.0";
                # 发布者名称
                option.Publisher = "babalae";
                # 应用程序名称
                option.AppName = "BetterGI";
                # 安装程序名称，包含本地化的卸载程序名称
                option.SetupName = $"BetterGI {Mui("UninstallProgram")}";
            })
            # 配置服务
            .UseServices(service =>
            {
                # 添加语言服务为单例
                service.AddSingleton<IMuiLanguageService, MuiLanguageService>();
                # 添加资源管理器服务为范围服务
                service.AddScoped<IExplorerService, ExplorerService>();
            })
            # 创建应用程序
            .CreateApp()
            # 配置多语言支持
            .UseMuiLanguage()
            # 配置主题
            .UseTheme(WindowsTheme.Auto)
            # 配置页面
            .UsePages(page =>
            {
                # 添加主页面
                page.Add(nameof(MainPage), typeof(MainPage));
                # 添加卸载页面
                page.Add(nameof(UninstallPage), typeof(UninstallPage));
                # 添加完成页面
                page.Add(nameof(FinishPage), typeof(FinishPage));
            })
            # 捕获调度程序未处理的异常
            .UseDispatcherUnhandledExceptionCatched()
            # 捕获域未处理的异常
            .UseDomainUnhandledExceptionCatched()
            # 捕获未观察的任务异常
            .UseUnobservedTaskExceptionCatched()
            # 运行应用程序
            .RunApp();
    }
}
```