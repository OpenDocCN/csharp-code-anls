# `.\better-genshin-impact\BetterGenshinImpact\GameTask\SystemControl.cs`

```cs
﻿using System; // 引入系统基础类库
using System.Diagnostics; // 引入用于进程和性能计数器的类库
using System.IO; // 引入用于文件和流的类库
using System.Threading.Tasks; // 引入异步编程的类库
using Vanara.PInvoke; // 引入 Vanara 的 PInvoke 库，用于 Windows API 调用

namespace BetterGenshinImpact.GameTask; // 定义命名空间

public class SystemControl
{
    // 查找并返回“原神”相关的窗口句柄
    public static nint FindGenshinImpactHandle()
    {
        // 调用 FindHandleByProcessName 方法，根据进程名查找窗口句柄
        return FindHandleByProcessName("YuanShen", "GenshinImpact", "Genshin Impact Cloud Game");
    }

    // 从本地启动游戏并异步等待窗口句柄的出现
    public static async Task<nint> StartFromLocalAsync(string path)
    {
        // 使用指定路径启动进程
        Process.Start(new ProcessStartInfo(path)
        {
            UseShellExecute = true, // 使用操作系统外壳启动进程
            Arguments = TaskContext.Instance().Config.GenshinStartConfig.GenshinStartArgs, // 传递启动参数
            WorkingDirectory = Path.GetDirectoryName(path) // 设置工作目录为路径的目录部分
        });

        // 尝试获取游戏窗口句柄，最多尝试 5 次，每次间隔不同的时间
        for (var i = 0; i < 5; i++)
        {
            var handle = FindGenshinImpactHandle(); // 查找游戏窗口句柄
            if (handle != 0) // 如果找到了有效的窗口句柄
            {
                await Task.Delay(2333); // 等待 2333 毫秒
                handle = FindGenshinImpactHandle(); // 再次查找游戏窗口句柄
                await Task.Delay(2577); // 等待 2577 毫秒
                return handle; // 返回找到的窗口句柄
            }

            await Task.Delay(5577); // 等待 5577 毫秒后再次尝试
        }
        // 如果超过 5 次都未找到窗口句柄，最后再尝试一次
        return FindGenshinImpactHandle();
    }

    // 检查游戏是否通过进程名激活
    public static bool IsGenshinImpactActiveByProcess()
    {
        var name = GetActiveProcessName(); // 获取当前活跃进程的名称
        // 判断进程名称是否为已知的游戏名称
        return name is "YuanShen" or "GenshinImpact" or "Genshin Impact Cloud Game";
    }

    // 检查游戏是否处于激活状态（通过窗口句柄）
    public static bool IsGenshinImpactActive()
    {
        var hWnd = User32.GetForegroundWindow(); // 获取当前前景窗口的句柄
        // 判断当前前景窗口句柄是否与游戏的句柄匹配
        return hWnd == TaskContext.Instance().GameHandle;
    }

    // 获取当前前景窗口的句柄
    public static nint GetForegroundWindowHandle()
    {
        return (nint)User32.GetForegroundWindow(); // 返回当前前景窗口的句柄
    }

    // 根据进程名称查找窗口句柄
    public static nint FindHandleByProcessName(params string[] names)
    {
        foreach (var name in names) // 遍历每个进程名称
        {
            var pros = Process.GetProcessesByName(name); // 根据进程名称获取进程列表
            if (pros.Length is not 0) // 如果找到至少一个进程
            {
                return pros[0].MainWindowHandle; // 返回第一个进程的主窗口句柄
            }
        }

        return 0; // 如果未找到匹配的进程，返回 0
    }

    // 根据窗口标题查找窗口句柄
    public static nint FindHandleByWindowName()
    {
        var handle = (nint)User32.FindWindow("UnityWndClass", "原神"); // 查找标题为“原神”的窗口句柄
        if (handle != 0) // 如果找到有效的窗口句柄
        {
            return handle; // 返回窗口句柄
        }

        handle = (nint)User32.FindWindow("UnityWndClass", "Genshin Impact"); // 查找标题为“Genshin Impact”的窗口句柄
        if (handle != 0) // 如果找到有效的窗口句柄
        {
            return handle; // 返回窗口句柄
        }

        handle = (nint)User32.FindWindow("Qt5152QWindowIcon", "云·原神"); // 查找标题为“云·原神”的窗口句柄
        if (handle != 0) // 如果找到有效的窗口句柄
        {
            return handle; // 返回窗口句柄
        }

        return 0; // 如果都未找到，返回 0
    }

    // 获取当前前景窗口的进程名称
    public static string? GetActiveProcessName()
    {
        try
        {
            var hWnd = User32.GetForegroundWindow(); // 获取当前前景窗口的句柄
            _ = User32.GetWindowThreadProcessId(hWnd, out var pid); // 获取窗口所属进程的 ID
            var p = Process.GetProcessById((int)pid); // 根据进程 ID 获取进程对象
            return p.ProcessName; // 返回进程名称
        }
        catch
        {
            return null; // 如果出现异常，返回 null
        }
    }

    public static Process? GetProcessByHandle(nint hWnd)
    {
        try
        {
            // 尝试获取窗口的进程ID，并将其存储在变量 pid 中
            _ = User32.GetWindowThreadProcessId(hWnd, out var pid);
            // 使用进程ID获取进程对象
            var p = Process.GetProcessById((int)pid);
            // 返回获取到的进程对象
            return p;
        }
        catch (Exception ex)
        {
            // 如果发生异常，将异常信息写入调试输出
            Debug.WriteLine(ex);
            // 返回 null 以表示失败
            return null;
        }
    }

    /// <summary>
    /// 获取窗口位置
    /// </summary>
    /// <param name="hWnd"></param>
    /// <returns></returns>
    public static RECT GetWindowRect(nint hWnd)
    {
        // 调用 DwmApi 获取窗口的扩展边界
        DwmApi.DwmGetWindowAttribute<RECT>(hWnd, DwmApi.DWMWINDOWATTRIBUTE.DWMWA_EXTENDED_FRAME_BOUNDS, out var windowRect);
        // 返回窗口的位置和尺寸信息
        return windowRect;
    }

    /// <summary>
    /// 游戏本身分辨率获取
    /// </summary>
    /// <param name="hWnd"></param>
    /// <returns></returns>
    public static RECT GetGameScreenRect(nint hWnd)
    {
        // 调用 User32 获取游戏窗口的客户区域
        User32.GetClientRect(hWnd, out var clientRect);
        // 返回游戏窗口的客户区域
        return clientRect;
    }

    /// <summary>
    /// GetWindowRect or GetGameScreenRect
    /// </summary>
    /// <param name="hWnd"></param>
    /// <returns></returns>
    public static RECT GetCaptureRect(nint hWnd)
    {
        // 获取窗口的矩形区域
        var windowRect = GetWindowRect(hWnd);
        // 获取游戏屏幕的矩形区域
        var gameScreenRect = GetGameScreenRect(hWnd);
        // 计算捕获矩形的左边界
        var left = windowRect.Left;
        // 计算捕获矩形的上边界
        var top = windowRect.Top + windowRect.Height - gameScreenRect.Height;
        // 计算捕获矩形的右边界
        var right = left + gameScreenRect.Width;
        // 计算捕获矩形的下边界
        var bottom = top + gameScreenRect.Height;
        // 返回计算得到的矩形区域
        return new RECT(left, top, right, bottom);
    }

    public static void ActivateWindow(nint hWnd)
    {
        // 还原窗口的显示状态
        User32.ShowWindow(hWnd, ShowWindowCommand.SW_RESTORE);
        // 将窗口设置为前景窗口
        User32.SetForegroundWindow(hWnd);
    }

    public static void ActivateWindow()
    {
        // 如果 TaskContext 尚未初始化，抛出异常
        if (!TaskContext.Instance().IsInitialized)
        {
            throw new Exception("请先启动BetterGI");
        }

        // 使用 TaskContext 的游戏句柄激活窗口
        ActivateWindow(TaskContext.Instance().GameHandle);
    }

    public static void Focus(nint hWnd)
    {
        // 如果窗口存在
        if (User32.IsWindow(hWnd))
        {
            // 发送消息恢复窗口状态
            _ = User32.SendMessage(hWnd, User32.WindowMessage.WM_SYSCOMMAND, User32.SysCommand.SC_RESTORE, 0);
            // 将窗口设置为前景窗口
            _ = User32.SetForegroundWindow(hWnd);
            // 循环直到窗口不再是图标化状态
            while (User32.IsIconic(hWnd))
            {
                continue;
            }

            // 将窗口置于顶部
            _ = User32.BringWindowToTop(hWnd);
        }
    }

    public static bool IsFullScreenMode(IntPtr hWnd)
    {
        // 如果窗口句柄无效，返回 false
        if (hWnd == IntPtr.Zero)
        {
            return false;
        }

        // 获取窗口的扩展样式
        var exStyle = User32.GetWindowLong(hWnd, User32.WindowLongFlags.GWL_EXSTYLE);

        // 判断窗口是否具有 WS_EX_TOPMOST 样式来判断是否为全屏模式
        return (exStyle & (int)User32.WindowStylesEx.WS_EX_TOPMOST) != 0;
    }

    // private static void StartFromLauncher(string path)
    // {
    //     // 通过launcher启动
    //     var process = Process.Start(new ProcessStartInfo(path) { UseShellExecute = true });
    //     Thread.Sleep(1000);
    //     // 获取launcher窗口句柄
    //     var hWnd = FindHandleByProcessName("launcher");
    //     var rect = GetWindowRect(hWnd);
    //     var dpiScale = Helpers.DpiHelper.ScaleY;
    //     // 对于launcher，启动按钮的位置时固定的，在launcher窗口的右下角
    //     Thread.Sleep(1000); // 等待 1 秒，确保窗口稳定
    //     Simulation.MouseEvent.Click((int)((float)rect.right * dpiScale) - (rect.Width / 5), (int)((float)rect.bottom * dpiScale) - (rect.Height / 8)); // 点击启动按钮的位置，考虑 DPI 缩放
    // }
    //
    // private static void StartCloudYaunShen(string path)
    // {
    //     // 通过launcher启动
    //     var process = Process.Start(new ProcessStartInfo(path) { UseShellExecute = true }); // 启动指定路径的程序
    //     Thread.Sleep(10000); // 等待 10 秒，确保程序启动完成
    //     // 获取launcher窗口句柄
    //     var hWnd = FindHandleByProcessName("Genshin Impact Cloud Game"); // 查找名为 "Genshin Impact Cloud Game" 的窗口句柄
    //     var rect = GetWindowRect(hWnd); // 获取窗口的矩形区域
    //     var dpiScale = Helpers.DpiHelper.ScaleY; // 获取 DPI 缩放因子
    //     // 对于launcher，启动按钮的位置时固定的，在launcher窗口的右下角
    //     Simulation.MouseEvent.Click(rect.right - (rect.Width / 6), rect.bottom - (rect.Height / 13 * 3)); // 点击窗口右下角的启动按钮，考虑 DPI 缩放
    //     // TODO：点完之后有个15s的倒计时，好像不处理也没什么问题，直接睡个20s吧
    //     Thread.Sleep(20000); // 等待 20 秒，确保启动过程完成
    // }
请提供需要注释的代码部分，以便我可以为每行代码添加注释。
```