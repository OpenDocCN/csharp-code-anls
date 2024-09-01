# `.\better-genshin-impact\Build\MicaSetup\Helper\System\RuntimeHelper.cs`

```cs
# 引入必要的命名空间
﻿using System;
using System.ComponentModel;
using System.Diagnostics;
using System.IO;
using System.Linq;
using System.Security.Principal;
using System.Threading;
using System.Threading.Tasks;

namespace MicaSetup.Helper;

# 定义静态类 RuntimeHelper
public static class RuntimeHelper
{
    # 静态属性，检查当前进程是否以管理员权限运行
    public static bool IsElevated { get; } = GetElevated();
    # 静态属性，检查调试器是否附加到当前进程
    public static bool IsDebuggerAttached => Debugger.IsAttached;
    # 静态属性，检查当前是否处于设计模式
    public static bool IsDesignMode { get; } = GetDesignMode();

    # 获取当前进程是否以管理员权限运行
    private static bool GetElevated()
    {
        # 获取当前 Windows 身份
        using WindowsIdentity identity = WindowsIdentity.GetCurrent();
        # 创建 WindowsPrincipal 对象，以确定是否具有管理员权限
        WindowsPrincipal principal = new(identity);
        # 检查当前用户是否在管理员角色中
        return principal.IsInRole(WindowsBuiltInRole.Administrator);
    }

    # 获取当前是否处于设计模式
    private static bool GetDesignMode()
    {
        # 如果使用模式是设计时模式，则返回 true
        if (LicenseManager.UsageMode == LicenseUsageMode.Designtime)
        {
            return true;
        }
        # 如果当前进程名为 "devenv"，即 Visual Studio 进程，返回 true
        else if (Process.GetCurrentProcess().ProcessName == "devenv")
        {
            return true;
        }
        # 否则返回 false
        return false;
    }

    # 确保当前进程以管理员权限运行
    public static void EnsureElevated()
    {
        # 如果调试器附加，记录警告并跳过提升操作
        if (IsDebuggerAttached)
        {
            Logger.Warn("[RuntimeHelper] IsDebuggerAttached causes skip EnsureElevated");
            return;
        }
        # 如果当前进程没有管理员权限，重新启动为管理员权限
        if (!IsElevated)
        {
            RestartAsElevated();
        }
    }

    # 重新格式化命令行参数
    public static string ReArguments()
    {
        # 获取命令行参数，跳过第一个参数（程序路径）
        string[] args = Environment.GetCommandLineArgs().Skip(1).ToArray();

        # 为每个参数添加引号
        for (int i = default; i < args.Length; i++)
        {
            args[i] = $@"""{args[i]}""";
        }
        # 将所有参数连接成一个字符串
        return string.Join(" ", args);
    }

    # 以管理员权限重新启动进程
    public static void RestartAsElevated(string fileName = null!, string dir = null!, string args = null!, int? exitCode = null, bool forced = false)
    {
        try
        {
            # 创建并配置新的进程
            FluentProcess.Create()
                # 设置要执行的文件名，如果未指定，则使用当前目录中的应用程序名称
                .FileName(fileName ?? Path.Combine(dir ?? AppDomain.CurrentDomain.BaseDirectory, AppDomain.CurrentDomain.FriendlyName))
                # 设置命令行参数，如果未指定，则使用重新格式化的参数
                .Arguments(args ?? ReArguments())
                # 设置工作目录，如果未指定，则使用当前目录
                .WorkingDirectory(dir ?? Environment.CurrentDirectory)
                # 使用外壳程序执行进程
                .UseShellExecute()
                # 设置进程为管理员权限
                .Verb("runas")
                # 启动进程
                .Start()
                .Forget();
        }
        # 捕获 Win32 异常（如用户拒绝权限提升）
        catch (Win32Exception)
        {
            return;
        }
        # 如果需要强制退出当前进程
        if (forced)
        {
            Process.GetCurrentProcess().Kill();
        }
        # 退出当前进程，并设置退出代码
        Environment.Exit(exitCode ?? 'r' + 'u' + 'n' + 'a' + 's');
    }

    # 检查是否为单例实例
    public static void CheckSingleInstance(string instanceName, Action<bool> callback = null!)
    {
        # 声明一个可空的 EventWaitHandle 变量
        EventWaitHandle? handle;

        try
        {
            # 尝试打开一个已存在的 EventWaitHandle 对象
            handle = EventWaitHandle.OpenExisting(instanceName);
            # 设置 EventWaitHandle 对象的状态
            handle.Set();
            # 如果 callback 不为空，调用它并传递 false
            callback?.Invoke(false);
            # 以 0xFFFF 状态码退出程序
            Environment.Exit(0xFFFF);
        }
        catch (WaitHandleCannotBeOpenedException)
        {
            # 如果无法打开 EventWaitHandle，调用 callback 并传递 true
            callback?.Invoke(true);
            # 创建一个新的 EventWaitHandle 对象
            handle = new EventWaitHandle(false, EventResetMode.AutoReset, instanceName);
        }
        # 确保在垃圾回收时保留对 handle 的引用
        GC.KeepAlive(handle);
        # 异步运行一个任务
        _ = Task.Run(() =>
        {
            # 在任务中循环，直到 EventWaitHandle 被触发
            while (handle.WaitOne())
            {
                # 在 UI 线程中激活并显示主界面
                UIDispatcherHelper.BeginInvoke(main =>
                {
                    main?.Activate();
                    main?.Show();
                });
            }
        });
    }
可以提供更多上下文或代码的前后部分吗？这样我可以更好地帮助你添加注释。
```