# `.\better-genshin-impact\BetterGenshinImpact\Helpers\RuntimeHelper.cs`

```cs
# 导入所需的命名空间
﻿using BetterGenshinImpact.Core.Config;
using Microsoft.Extensions.Hosting;
using System;
using System.ComponentModel;
using System.Diagnostics;
using System.IO;
using System.Linq;
using System.Security.Principal;
using System.Text;
using System.Threading;
using System.Threading.Tasks;

namespace BetterGenshinImpact.Helpers;

# 定义内部静态类 RuntimeHelper
internal static class RuntimeHelper
{
    # 获取当前是否以管理员权限运行的属性
    public static bool IsElevated { get; } = GetElevated();
    # 获取当前是否附加调试器的属性
    public static bool IsDebuggerAttached => Debugger.IsAttached;
    # 获取当前是否在设计模式下运行的属性
    public static bool IsDesignMode { get; } = GetDesignMode();

    # 获取当前是否为调试模式的属性
    public static bool IsDebug =>
#if DEBUG
        true;

#else
        false;
#endif

    # 获取当前是否以管理员权限运行的私有方法
    private static bool GetElevated()
    {
        using WindowsIdentity identity = WindowsIdentity.GetCurrent();
        WindowsPrincipal principal = new(identity);
        # 判断当前用户是否为管理员
        return principal.IsInRole(WindowsBuiltInRole.Administrator);
    }

    # 获取当前是否在设计模式下运行的私有方法
    private static bool GetDesignMode()
    {
        # 判断许可证使用模式是否为设计时模式
        if (LicenseManager.UsageMode == LicenseUsageMode.Designtime)
        {
            return true;
        }
        # 判断当前进程名称是否为 "devenv" (Visual Studio)
        else if (Process.GetCurrentProcess().ProcessName == "devenv")
        {
            return true;
        }
        return false;
    }

    # 确保当前进程以管理员权限运行的方法
    public static void EnsureElevated()
    {
        # 如果当前不是以管理员权限运行，则重新以管理员权限启动
        if (!IsElevated)
        {
            RestartAsElevated();
        }
    }

    # 将命令行参数格式化为带引号的字符串的方法
    public static string ReArguments()
    {
        string[] args = Environment.GetCommandLineArgs().Skip(1).ToArray();

        # 为每个参数添加引号
        for (int i = default; i < args.Length; i++)
        {
            args[i] = $@"""{args[i]}""";
        }
        # 将参数数组连接成一个字符串
        return string.Join(" ", args);
    }

    # 以管理员权限重新启动应用程序的方法
    public static void RestartAsElevated(string fileName = null!, string dir = null!, string args = null!, int? exitCode = null, bool forced = false)
    {
        try
        {
            # 配置启动信息
            ProcessStartInfo startInfo = new()
            {
                UseShellExecute = true,
                WorkingDirectory = dir ?? Global.StartUpPath,
                FileName = fileName ?? "BetterGI.exe",
                Arguments = args ?? ReArguments(),
                Verb = "runas"
            };
            try
            {
                # 启动新的进程
                _ = Process.Start(startInfo);
            }
            catch (Exception ex)
            {
                # 捕获启动失败异常，记录错误并显示消息
                Debug.WriteLine(ex);
                MessageBox.Error("以管理员权限启动 BetterGI 失败，非管理员权限下所有模拟操作功能均不可用！\r\n请尝试 右键 —— 以管理员身份运行 的方式启动 BetterGI");
                return;
            }
        }
        catch (Win32Exception)
        {
            return;
        }
        # 如果需要强制关闭当前进程，则结束当前进程
        if (forced)
        {
            Process.GetCurrentProcess().Kill();
        }
        # 退出当前进程并设置退出代码
        Environment.Exit(exitCode ?? 'r' + 'u' + 'n' + 'a' + 's');
    }

    # 检查是否存在单例实例的方法，回调参数可选
    public static void CheckSingleInstance(string instanceName, Action<bool> callback = null!)
    # 定义一个可空的 EventWaitHandle 对象
    {
        EventWaitHandle? handle;

        # 尝试打开一个已存在的 EventWaitHandle 实例
        try
        {
            handle = EventWaitHandle.OpenExisting(instanceName);
            # 设置 EventWaitHandle 对象的状态
            handle.Set();
            # 如果 callback 不为空，则调用它并传入 false
            callback?.Invoke(false);
            # 退出应用程序，使用 0xFFFF 作为退出代码
            Environment.Exit(0xFFFF);
        }
        # 捕获无法打开的异常
        catch (WaitHandleCannotBeOpenedException)
        {
            # 如果 callback 不为空，则调用它并传入 true
            callback?.Invoke(true);
            # 创建一个新的 EventWaitHandle 实例
            handle = new EventWaitHandle(false, EventResetMode.AutoReset, instanceName);
        }

        # 启动一个新的任务
        Task.Factory.StartNew(() =>
        {
            # 在 handle 被信号唤醒时循环
            while (handle.WaitOne())
            {
#if false
                # 如果条件为 false，下面的代码块不会被执行
                UIDispatcherHelper.BeginInvoke(main =>
                {
                    # 在主线程中激活窗口
                    main?.Activate();
                    # 显示窗口
                    main?.Show();
                });
#endif
            }
        }, TaskCreationOptions.LongRunning);
    }

    # 静态方法用于检查应用程序是否集成完整
    public static void CheckIntegration()
    {
        # 如果指定目录不存在，则认为关键文件缺失
        if (!Directory.Exists(Global.Absolute("Assets")) || !Directory.Exists(Global.Absolute("GameTask")))
        {
            # 创建一个字符串生成器，初始内容为“发现有关键文件缺失，”
            StringBuilder stringBuilder = new("发现有关键文件缺失，");
            # 根据当前目录是否为启动路径，附加不同的提示信息
            stringBuilder.Append(Environment.GetFolderPath(Environment.SpecialFolder.DesktopDirectory) == Global.StartUpPath
                ? "请不要把主程序exe文件剪切到桌面。正确的做法：请右键点击主程序，在弹出的菜单中选择“发送到”选项，然后选择“桌面创建快捷方式”。"
                : "请重新安装软件");

            # 显示警告消息框
            MessageBox.Warning(stringBuilder.ToString());
            # 退出程序，状态码为 0xFFFF
            Environment.Exit(0xFFFF);
        }
    }
}

internal static class RuntimeExtension
{
    # 扩展方法：确保应用程序以管理员权限运行
    public static IHostBuilder UseElevated(this IHostBuilder app)
    {
        # 调用 RuntimeHelper 方法以确保管理员权限
        RuntimeHelper.EnsureElevated();
        # 返回 IHostBuilder 对象
        return app;
    }

    # 扩展方法：确保应用程序为单实例
    public static IHostBuilder UseSingleInstance(this IHostBuilder self, string instanceName, Action<bool> callback = null!)
    {
        # 调用 RuntimeHelper 方法检查单实例状态，并执行回调
        RuntimeHelper.CheckSingleInstance(instanceName, callback);
        # 返回 IHostBuilder 对象
        return self;
    }

    # 扩展方法：检查应用程序集成完整性
    public static IHostBuilder CheckIntegration(this IHostBuilder app)
    {
        # 调用 RuntimeHelper 方法检查集成
        RuntimeHelper.CheckIntegration();
        # 返回 IHostBuilder 对象
        return app;
    }
}
```