# `.\better-genshin-impact\Build\MicaSetup\Program.cs`

```cs
# 引用 MicaSetup 设计控件命名空间
﻿using MicaSetup.Design.Controls;
# 引用 MicaSetup 服务命名空间
using MicaSetup.Services;
# 引用 MicaSetup 视图命名空间
using MicaSetup.Views;
# 引用 Microsoft.Extensions.DependencyInjection 命名空间，用于依赖注入
using Microsoft.Extensions.DependencyInjection;
# 引用系统基本功能命名空间
using System;
# 引用程序集反射命名空间
using System.Reflection;
# 引用运行时互操作命名空间，用于与非托管代码交互
using System.Runtime.InteropServices;

# 设置程序集的唯一标识符（GUID）
[assembly: Guid("00000000-0000-0000-0000-000000000000")]
# 设置程序集标题
[assembly: AssemblyTitle("BetterGI Setup")]
# 设置程序集产品名称
[assembly: AssemblyProduct("BetterGI")]
# 设置程序集描述
[assembly: AssemblyDescription("BetterGI Setup")]
# 设置程序集公司名称
[assembly: AssemblyCompany("Lemutec")]
# 设置程序集版权信息
[assembly: AssemblyCopyright("Under GPL-3.0 license. Copyright (c) better-genshin-impact Contributors.")]
# 设置程序集版本
[assembly: AssemblyVersion("2.0.0.0")]
# 设置文件版本
[assembly: AssemblyFileVersion("2.0.0.0")]

# 定义 MicaSetup 命名空间
namespace MicaSetup;

# 定义一个内部的程序类
internal class Program
{
    # 设置主线程单线程单元（STA），并定义主方法
    [STAThread]
    internal static void Main()
    {
        # 创建应用程序构建器
        Hosting.CreateBuilder()
            # 启用日志记录
            .UseLogger()
            # 设置为单实例应用
            .UseSingleInstance("BetterGI_MicaSetup")
            # 使用临时路径分叉
            .UseTempPathFork()
            # 以提升权限运行
            .UseElevated()
            # 启用 DPI 感知
            .UseDpiAware()
            # 配置应用程序选项
            .UseOptions(option =>
            {
                # 是否创建桌面快捷方式
                option.IsCreateDesktopShortcut = true;
                # 是否创建卸载程序
                option.IsCreateUninst = true;
                # 是否创建开始菜单快捷方式
                option.IsCreateStartMenu = true;
                # 是否创建快速启动快捷方式
                option.IsCreateQuickLaunch = false;
                # 是否创建注册表项
                option.IsCreateRegistryKeys = true;
                # 是否设置为开机自启
                option.IsCreateAsAutoRun = false;
                # 是否自定义可见的开机自启
                option.IsCustomizeVisiableAutoRun = false;
                # 开机自启时的启动命令
                option.AutoRunLaunchCommand = "-autostart";
                # 是否使用经典文件夹选择器
                option.UseFolderPickerPreferClassic = false;
                # 是否偏好使用 x86 安装路径
                option.UseInstallPathPreferX86 = false;
                # 注册表是否偏好 x86
                option.IsUseRegistryPreferX86 = null!;
                # 是否允许完整的文件夹安全性
                option.IsAllowFullFolderSecurity = true;
                # 是否允许防火墙
                option.IsAllowFirewall = true;
                # 是否刷新资源管理器
                option.IsRefreshExplorer = true;
                # 是否安装证书
                option.IsInstallCertificate = false;
                # 覆盖安装移除的扩展名
                option.OverlayInstallRemoveExt = "exe,dll,pdb";
                # 解包密码
                option.UnpackingPassword = null!;
                # 可执行文件的名称
                option.ExeName = @"BetterGI\BetterGI.exe";
                # 应用程序的键名称
                option.KeyName = "BetterGI";
                # 应用程序的显示名称
                option.DisplayName = "BetterGI";
                # 应用程序的显示图标
                option.DisplayIcon = @"BetterGI\BetterGI.exe";
                # 应用程序的显示版本
                option.DisplayVersion = "0.0.0.0";
                # 发布者名称
                option.Publisher = "babalae";
                # 应用程序名称
                option.AppName = "BetterGI";
                # 安装程序名称
                option.SetupName = $"BetterGI {Mui("Setup")}";
            })
            # 配置服务
            .UseServices(service =>
            {
                # 添加语言服务
                service.AddSingleton<IMuiLanguageService, MuiLanguageService>();
                # 添加 .NET 版本服务
                service.AddScoped<IDotNetVersionService, DotNetVersionService>();
                # 添加资源管理器服务
                service.AddScoped<IExplorerService, ExplorerService>();
            })
            # 创建应用程序
            .CreateApp()
            # 启用 MUI 语言支持
            .UseMuiLanguage()
            # 设置主题
            .UseTheme(WindowsTheme.Auto)
            # 配置页面
            .UsePages(page =>
            {
                # 添加主页面
                page.Add(nameof(MainPage), typeof(MainPage));
                # 添加安装页面
                page.Add(nameof(InstallPage), typeof(InstallPage));
                # 添加完成页面
                page.Add(nameof(FinishPage), typeof(FinishPage));
            })
            # 捕获调度程序未处理的异常
            .UseDispatcherUnhandledExceptionCatched()
            # 捕获域未处理的异常
            .UseDomainUnhandledExceptionCatched()
            # 捕获未观察到的任务异常
            .UseUnobservedTaskExceptionCatched()
            # 运行应用程序
            .RunApp();
    }
# 代码块的结束括号，标志着代码结构的结束
}
```