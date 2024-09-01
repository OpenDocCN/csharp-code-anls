# `.\better-genshin-impact\Fischless.HotkeyCapture\HotkeyHook.cs`

```cs
# 引入运行时互操作服务的命名空间
﻿using System.Runtime.InteropServices;
# 引入 Vanara.PInvoke 命名空间，用于调用 Win32 API
using Vanara.PInvoke;

namespace Fischless.HotkeyCapture;

# 定义一个不可继承的 HotkeyHook 类，实现 IDisposable 接口以便于资源管理
public sealed class HotkeyHook : IDisposable
{
    # 定义一个事件，用于处理按键事件
    public event EventHandler<KeyPressedEventArgs>? KeyPressed = null;

    # 创建一个 Window 实例用于接收系统消息
    private readonly Window window = new();
    # 当前注册的热键 ID
    private int currentId;

    # 定义一个内部类 Window，继承自 NativeWindow，并实现 IDisposable 接口
    private class Window : NativeWindow, IDisposable
    {
        # 定义一个事件，用于处理按键事件
        public event EventHandler<KeyPressedEventArgs>? KeyPressed = null;

        # Window 类的构造函数
        public Window()
        {
            # 创建窗口句柄
            CreateHandle(new CreateParams());
        }

        # 重写窗口过程方法以处理消息
        protected override void WndProc(ref Message m)
        {
            base.WndProc(ref m);

            # 判断是否为热键消息
            if (m.Msg == (int)User32.WindowMessage.WM_HOTKEY)
            {
                # 从消息参数中提取键值和修饰符
                Keys key = (Keys)(((int)m.LParam >> 16) & 0xFFFF);
                User32.HotKeyModifiers modifier = (User32.HotKeyModifiers)((int)m.LParam & 0xFFFF);

                # 触发 KeyPressed 事件
                KeyPressed?.Invoke(this, new KeyPressedEventArgs(modifier, key));
            }
        }

        # 实现 IDisposable 接口的 Dispose 方法以释放资源
        public void Dispose()
        {
            # 销毁窗口句柄
            DestroyHandle();
        }
    }

    # HotkeyHook 类的构造函数
    public HotkeyHook()
    {
        # 订阅内部 Window 实例的 KeyPressed 事件
        window.KeyPressed += (sender, args) =>
        {
            # 触发 HotkeyHook 类的 KeyPressed 事件
            KeyPressed?.Invoke(this, args);
        };
    }

    # 注册热键的方法
    public void RegisterHotKey(User32.HotKeyModifiers modifier, Keys key)
    {
        # 增加当前热键 ID
        currentId += 1;
        # 调用 Win32 API 注册热键
        if (!User32.RegisterHotKey(window!.Handle, currentId, modifier, (uint)key))
        {
            # 检查错误代码，如果热键已注册则抛出异常
            if (Marshal.GetLastWin32Error() == SystemErrorCodes.ERROR_HOTKEY_ALREADY_REGISTERED)
            {
                throw new InvalidOperationException("Hotkey already registered");
            }
            else
            {
                # 否则抛出注册失败异常
                throw new InvalidOperationException("Hotkey registration failed");
            }
        }
    }

    # 注销热键的方法
    public void UnregisterHotKey()
    {
        # 遍历并注销所有注册的热键
        for (int i = currentId; i > 0; i--)
        {
            User32.UnregisterHotKey(window!.Handle, i);
        }
    }

    # 实现 IDisposable 接口的 Dispose 方法以释放资源
    public void Dispose()
    {
        # 注销所有热键
        UnregisterHotKey();
        # 释放窗口资源
        window?.Dispose();
    }
}
```