# `.\better-genshin-impact\BetterGenshinImpact\GameTask\AutoWood\Utils\Login3rdParty.cs`

```cs
# 引用必要的命名空间和库
﻿using BetterGenshinImpact.Core.Recognition.OpenCv;  # 引用 OpenCV 相关功能
using BetterGenshinImpact.Helpers.Extensions;  # 引用辅助扩展方法
using System;  # 引用基础系统功能
using System.Diagnostics;  # 引用进程相关功能
using System.IO;  # 引用文件和流功能
using System.Linq;  # 引用 LINQ 扩展方法
using System.Runtime.InteropServices;  # 引用用于调用非托管代码的功能
using System.Text;  # 引用文本处理功能
using System.Threading;  # 引用线程功能
using Vanara.PInvoke;  # 引用 Vanara PInvoke 库
using static BetterGenshinImpact.GameTask.Common.TaskControl;  # 引用任务控制相关功能

namespace BetterGenshinImpact.GameTask.AutoWood.Utils;  # 定义命名空间

internal sealed class Login3rdParty  # 定义一个内部密封类
{
    public enum The3rdPartyType  # 定义一个枚举类型表示第三方类型
    {
        None,  # 没有第三方
        Bilibili,  # Bilibili 第三方
    }

    public bool IsAvailabled => Type != The3rdPartyType.None;  # 判断当前第三方类型是否可用
    public The3rdPartyType Type { get; private set; } = default;  # 定义第三方类型属性，初始值为默认值

    public void RefreshAvailabled()  # 刷新第三方可用性
    {
        Type = The3rdPartyType.None;  # 重置第三方类型为 None

        try  # 开始异常处理块
        {
            if (Process.GetProcessesByName("YuanShen").FirstOrDefault() is Process p)  # 查找名为 "YuanShen" 的进程，如果找到则执行
            {
                uint tid = User32.GetWindowThreadProcessId(p.MainWindowHandle, out uint pid);  # 获取进程主窗口线程 ID 和进程 ID

                if (tid != 0)  # 如果线程 ID 不为 0，则执行
                {
                    using Kernel32.SafeHPROCESS hProcess = Kernel32.OpenProcess(new ACCESS_MASK(Kernel32.ProcessAccess.PROCESS_QUERY_INFORMATION), false, pid);  # 打开进程以查询信息

                    if (!hProcess.IsInvalid)  # 如果打开的进程句柄有效，则执行
                    {
                        StringBuilder devicePath = new(260);  # 创建一个 StringBuilder 实例用于存储进程镜像路径
                        uint size = (uint)devicePath.Capacity;  # 获取 StringBuilder 的容量作为路径缓冲区大小

                        if (Kernel32.QueryFullProcessImageName(hProcess, 0, devicePath, ref size))  # 查询进程镜像完整路径
                        {
                            FileInfo fileInfo = new(devicePath.ToString());  # 创建 FileInfo 实例表示进程镜像文件信息

                            if (fileInfo.Exists)  # 如果文件存在，则执行
                            {
                                string? configIni = Path.Combine(fileInfo.DirectoryName!, "config.ini");  # 构建 config.ini 文件路径
                                string[] lines = File.ReadAllLines(configIni);  # 读取 config.ini 文件的所有行

                                foreach (string line in lines)  # 遍历文件中的每一行
                                {
                                    string kv = line.Trim();  # 去除行首尾的空白字符
                                    if (kv.StartsWith("channel=") && kv.EndsWith("14"))  # 如果行以 "channel=" 开头并以 "14" 结尾，则执行
                                    {
                                        Type = The3rdPartyType.Bilibili;  # 设置第三方类型为 Bilibili
                                        break;  # 退出循环
                                    }
                                }
                            }
                        }
                        else  # 如果查询进程镜像路径失败，则执行
                        {
                            Debug.WriteLine($"Error getting process image file name. Error code: {Marshal.GetLastWin32Error()}");  # 输出错误信息和错误代码
                        }
                    }
                }
            }
        }
        catch  # 捕获异常
        {
            /// 处理异常但不做任何操作
        }
    }

    public void Login(CancellationTokenSource cts)  # 登录方法，接收一个取消令牌源
    {
        int failCount = default; // 初始化失败计数器为默认值（通常为0）

        while (!LoginPrivate(cts)) // 循环直到 LoginPrivate 方法返回 true
        {
            // 需要支持可退出的尝试。
            // 当超过 20 次尝试时退出尝试。
            if (++failCount > 20) // 失败计数器加1，并检查是否超过20次
            {
                Debug.WriteLine("[AutoWood] Give up to check login button and don't try again."); // 输出放弃检查登录按钮的调试信息
                break; // 退出循环
            }
            Debug.WriteLine($"[AutoWood] Fail to check login button {failCount} time(s)."); // 输出当前失败次数的调试信息
            Sleep(500, cts); // 暂停500毫秒，支持取消操作
        }
        Debug.WriteLine("[AutoWood] Exit while check login button."); // 输出退出循环的调试信息
    }

    private bool LoginPrivate(CancellationTokenSource cts) // 私有方法，用于尝试登录操作
    {
        if (Type == The3rdPartyType.Bilibili) // 如果类型是 Bilibili
        {
            if (Process.GetProcessesByName("YuanShen").FirstOrDefault() is Process process) // 查找名为 "YuanShen" 的进程，并获取第一个实例
            {
                if (GetBHWnd(process) != IntPtr.Zero) // 如果获取到有效的窗口句柄
                {
                    // 只是为了处理 WebUI 的渐变效果
                    Sleep(4000, cts); // 暂停4000毫秒，支持取消操作

                    var p = TaskContext.Instance() // 获取任务上下文实例
                        .SystemInfo.CaptureAreaRect.GetCenterPoint() // 获取捕获区域的中心点
                        .Add(new(0, 85)); // 将中心点向下移动85个像素

                    p.Click(); // 点击该位置
                    Debug.WriteLine("[AutoWood] Click login button for the B one"); // 输出点击登录按钮的调试信息

                    // 只是为了处理 WebUI 的渐变效果
                    Sleep(3000, cts); // 暂停3000毫秒，支持取消操作

                    if (GetBHWnd(process) != IntPtr.Zero) // 如果再次获取到有效的窗口句柄
                    {
                        p.Click(); // 再次点击该位置
                        Debug.WriteLine("[AutoWood] Click login button for the B one [LAST CHANCE]"); // 输出最后一次点击登录按钮的调试信息
                    }

                    return true; // 返回成功状态
                }
                return false; // 如果没有找到有效的窗口句柄，则返回失败状态
            }
            return false; // 如果没有找到名为 "YuanShen" 的进程，则返回失败状态
        }
        else
        {
            // 忽略并以 OK 退出
            return true; // 返回成功状态
        }
    }

    static nint GetBHWnd(Process process) // 静态方法，用于获取指定进程的窗口句柄
    {
        nint bHWnd = IntPtr.Zero; // 初始化窗口句柄为零

        _ = User32.EnumWindows((HWND hWnd, nint lParam) => // 枚举所有窗口，并对每个窗口执行指定的回调
        {
            try
            {
                _ = User32.GetWindowThreadProcessId(hWnd, out uint pid); // 获取窗口所属的进程 ID

                if (pid == process.Id) // 如果进程 ID 匹配
                {
                    int capacity = User32.GetWindowTextLength(hWnd); // 获取窗口标题长度
                    StringBuilder title = new(capacity + 1); // 创建一个足够大的 StringBuilder 实例
                    _ = User32.GetWindowText(hWnd, title, title.Capacity); // 获取窗口标题

                    Debug.WriteLine($"[AutoWood] Enum Windows result is {title}"); // 输出窗口标题的调试信息
                    if (title.ToString().Contains("bilibili", StringComparison.OrdinalIgnoreCase)) // 如果标题包含 "bilibili"（忽略大小写）
                    {
                        bHWnd = (nint)hWnd; // 设置找到的窗口句柄
                        return false; // 退出枚举窗口的回调
                    }
                }
            }
            catch
            {
                ///
            }
            return true; // 继续枚举窗口
        }, IntPtr.Zero); // 传递一个空的参数给回调

        return bHWnd; // 返回找到的窗口句柄
    }
这段代码似乎不完整，能否提供更多代码或上下文？
```