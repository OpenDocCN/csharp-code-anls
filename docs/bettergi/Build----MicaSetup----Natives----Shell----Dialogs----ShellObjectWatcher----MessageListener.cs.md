# `.\better-genshin-impact\Build\MicaSetup\Natives\Shell\Dialogs\ShellObjectWatcher\MessageListener.cs`

```cs
﻿using MicaSetup.Natives;  // 引入 MicaSetup.Natives 命名空间
using System;  // 引入 System 命名空间
using System.Collections.Generic;  // 引入 System.Collections.Generic 命名空间
using System.Runtime.InteropServices;  // 引入 System.Runtime.InteropServices 命名空间
using System.Threading;  // 引入 System.Threading 命名空间
using static MicaSetup.Natives.User32;  // 引入 User32 类中的静态成员

namespace MicaSetup.Shell.Dialogs;  // 定义命名空间 MicaSetup.Shell.Dialogs

#pragma warning disable CS8618  // 禁用 CS8618 警告，通常用于非空性警告

internal class MessageListener : IDisposable  // 定义 MessageListener 类，实现 IDisposable 接口
{
    public const uint CreateWindowMessage = (uint)WindowMessage.WM_USER + 1;  // 定义创建窗口消息常量
    public const uint DestroyWindowMessage = (uint)WindowMessage.WM_USER + 2;  // 定义销毁窗口消息常量
    public const uint BaseUserMessage = (uint)WindowMessage.WM_USER + 5;  // 定义基础用户消息常量

    private const string MessageWindowClassName = "MessageListenerClass";  // 定义消息窗口类名

    private static readonly object _threadlock = new();  // 定义线程锁对象
    private static uint _atom;  // 定义用于窗口类原子标识符的静态字段
    private static Thread _windowThread = null!;  // 定义窗口线程的静态字段，并初始化为 null
    private static volatile bool _running = false;  // 定义标志窗口是否运行的静态字段

    private static readonly ShellObjectWatcherNativeMethods.WndProcDelegate wndProc = WndProc;  // 定义窗口过程委托，指向 WndProc 方法
    private static readonly Dictionary<IntPtr, MessageListener> _listeners = new();  // 定义保存窗口句柄到 MessageListener 实例的字典
    private static nint _firstWindowHandle = 0;  // 定义第一个窗口句柄的静态字段

    private static readonly object _crossThreadWindowLock = new();  // 定义跨线程窗口锁对象
    private static nint _tempHandle = 0;  // 定义临时窗口句柄的静态字段

    public event EventHandler<WindowMessageEventArgs> MessageReceived;  // 定义消息接收事件

    public MessageListener()  // MessageListener 类的构造函数
    {
        lock (_threadlock)  // 锁定线程锁
        {
            if (_windowThread == null)  // 如果窗口线程为 null
            {
                _windowThread = new Thread(ThreadMethod);  // 创建新的线程，并指定线程方法
                _windowThread.SetApartmentState(ApartmentState.STA);  // 设置线程的单元状态为 STA
                _windowThread.Name = "ShellObjectWatcherMessageListenerHelperThread";  // 设置线程名称

                lock (_crossThreadWindowLock)  // 锁定跨线程窗口锁
                {
                    _windowThread.Start();  // 启动线程
                    Monitor.Wait(_crossThreadWindowLock);  // 等待跨线程窗口锁
                }

                _firstWindowHandle = WindowHandle;  // 获取第一个窗口句柄
            }
            else  // 如果窗口线程不为 null
            {
                CrossThreadCreateWindow();  // 调用跨线程创建窗口的方法
            }

            if (WindowHandle == 0)  // 如果窗口句柄为 0
            {
                throw new ShellException(LocalizedMessages.MessageListenerCannotCreateWindow,  // 抛出 ShellException 异常
                    Marshal.GetExceptionForHR(Marshal.GetHRForLastWin32Error()));  // 包含最后一个 Win32 错误的 HRESULT 错误信息
            }

            _listeners.Add(WindowHandle, this);  // 将当前实例添加到监听器字典中，键为窗口句柄
        }
    }

    private void CrossThreadCreateWindow()  // 跨线程创建窗口的方法
    {
        if (_firstWindowHandle == 0)  // 如果第一个窗口句柄为 0
        {
            throw new InvalidOperationException(LocalizedMessages.MessageListenerNoWindowHandle);  // 抛出 InvalidOperationException 异常
        }

        lock (_crossThreadWindowLock)  // 锁定跨线程窗口锁
        {
            User32.PostMessage(_firstWindowHandle, (int)CreateWindowMessage, 0, 0);  // 向第一个窗口句柄发送创建窗口消息
            Monitor.Wait(_crossThreadWindowLock);  // 等待跨线程窗口锁
        }

        WindowHandle = _tempHandle;  // 设置窗口句柄为临时窗口句柄
    }

    private static void RegisterWindowClass()  // 注册窗口类的方法
    {
        // 创建并初始化一个 WindowClassEx 结构体实例
        WindowClassEx classEx = new()
        {
            // 设置窗口类名
            ClassName = MessageWindowClassName,
            // 设置窗口过程函数
            WndProc = wndProc,
            // 设置结构体的大小
            Size = (uint)Marshal.SizeOf(typeof(WindowClassEx)),
        };

        // 注册窗口类，并获取其原子标识符
        var atom = ShellObjectWatcherNativeMethods.RegisterClassEx(ref classEx);
        // 如果注册失败，则抛出异常
        if (atom == 0)
        {
            throw new ShellException(LocalizedMessages.MessageListenerClassNotRegistered,
                // 获取并包装上一个 Windows 错误的异常
                Marshal.GetExceptionForHR(Marshal.GetHRForLastWin32Error()));
        }
        // 保存窗口类的原子标识符
        _atom = atom;
    }

    private static nint CreateWindow()
    {
        // 创建一个窗口，并返回其句柄
        nint handle = ShellObjectWatcherNativeMethods.CreateWindowEx(
            0, // 扩展样式
            MessageWindowClassName, // 窗口类名
            "MessageListenerWindow", // 窗口标题
            0, // 窗口样式
            0, 0, 0, 0, // 窗口位置和尺寸
            new IntPtr(-3), // 父窗口句柄
            0, // 菜单句柄
            0, // 实例句柄
            0); // 附加参数

        // 返回创建的窗口句柄
        return handle;
    }

    private void ThreadMethod()
    {
        // 锁定跨线程窗口锁
        lock (_crossThreadWindowLock)
        {
            // 标记正在运行
            _running = true;
            // 如果窗口类尚未注册，则进行注册
            if (_atom == 0)
            {
                RegisterWindowClass();
            }
            // 创建窗口并保存其句柄
            WindowHandle = CreateWindow();

            // 释放锁，并通知等待的线程
            Monitor.Pulse(_crossThreadWindowLock);
        }

        // 处理消息循环，直到运行标记被设置为 false
        while (_running)
        {
            // 如果有消息到达，则获取并分派消息
            if (ShellObjectWatcherNativeMethods.GetMessage(out Message msg, 0, 0, 0))
            {
                ShellObjectWatcherNativeMethods.DispatchMessage(ref msg);
            }
        }
    }

    private static int WndProc(nint hwnd, uint msg, nint wparam, nint lparam)
    {
        // 处理窗口消息
        switch (msg)
        {
            case CreateWindowMessage:
                // 处理创建窗口消息
                lock (_crossThreadWindowLock)
                {
                    // 创建临时窗口句柄
                    _tempHandle = CreateWindow();
                    // 释放锁，并通知等待的线程
                    Monitor.Pulse(_crossThreadWindowLock);
                }
                break;

            case (uint)WindowMessage.WM_DESTROY:
                // 处理窗口销毁消息，停止运行
                _running = false;
                break;

            default:
                // 处理其他消息
                MessageListener listener;
                // 尝试从字典中获取对应的消息监听器
                if (_listeners.TryGetValue(hwnd, out listener))
                {
                    // 创建并封装消息对象
                    Message message = new(hwnd, msg, wparam, lparam, 0, new POINT());
                    // 调用监听器的消息接收事件
                    listener.MessageReceived.SafeRaise(listener, new WindowMessageEventArgs(message));
                }
                break;
        }

        // 调用默认窗口过程
        return ShellObjectWatcherNativeMethods.DefWindowProc(hwnd, msg, wparam, lparam);
    }

    public nint WindowHandle { get; private set; } // 窗口句柄属性

    public static bool Running
    { get { return _running; } } // 运行状态属性

    ~MessageListener()
    {
        // 析构函数，调用释放资源方法
        Dispose(false);
    }

    public void Dispose()
    {
        // 释放资源
        Dispose(true);
        // 抑制终结器调用
        GC.SuppressFinalize(this);
    }

    protected virtual void Dispose(bool disposing)
    # 判断是否正在处理释放资源
    {
        if (disposing)
        {
            # 获取线程锁以确保线程安全
            lock (_threadlock)
            {
                # 从监听器列表中移除当前窗口句柄
                _listeners.Remove(WindowHandle);
                # 如果监听器列表为空
                if (_listeners.Count == 0)
                {
                    # 向窗口发送销毁消息
                    User32.PostMessage(WindowHandle, (int)WindowMessage.WM_DESTROY, 0, 0);
                }
            }
        }
    }
} 
// 结束了前一个代码块或类定义

public class WindowMessageEventArgs : EventArgs
{
    // 声明一个只读属性 Message，用于存储消息
    public Message Message { get; private set; }

    // 构造函数，初始化 Message 属性
    internal WindowMessageEventArgs(Message msg) => Message = msg;
    // 将传入的 Message 对象赋值给属性 Message
}
```