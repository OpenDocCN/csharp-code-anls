# `.\better-genshin-impact\Fischless.KeyboardCapture\KeyboardHook.cs`

```cs
﻿using System.Diagnostics;  // 引入用于进程和诊断的类
using System.Runtime.InteropServices;  // 引入用于平台调用的类和方法
using Vanara.PInvoke;  // 引入 Vanara.PInvoke 库以进行 Windows API 调用

namespace Fischless.KeyboardCapture;  // 定义命名空间

public sealed class KeyboardHook : IDisposable  // 定义一个封闭的类实现 IDisposable 接口
{
    public event KeyboardEventHandler KeyDown = null!;  // 定义一个键按下事件，事件处理程序类型为 KeyboardEventHandler

    public event KeyboardPressEventHandler KeyPress = null!;  // 定义一个键按下并释放事件，事件处理程序类型为 KeyboardPressEventHandler

    public event KeyboardEventHandler KeyUp = null!;  // 定义一个键释放事件，事件处理程序类型为 KeyboardEventHandler

    private User32.SafeHHOOK hook = new(IntPtr.Zero);  // 声明并初始化一个安全的钩子句柄，初始值为 IntPtr.Zero（空值）

    private User32.HookProc? hookProc;  // 声明一个钩子回调方法的委托，初始化为空

    ~KeyboardHook()  // 析构函数
    {
        Dispose();  // 调用 Dispose 方法进行资源清理
    }

    public void Dispose()  // 实现 IDisposable 接口的 Dispose 方法
    {
        Stop();  // 调用 Stop 方法以停止钩子
    }

    public void Start()  // 启动键盘钩子的设置
    {
        if (hook.IsNull)  // 检查钩子是否为空（即是否已安装）
        {
            hookProc = KeyboardHookProc;  // 设置钩子的回调函数为 KeyboardHookProc
            // 安装全局键盘钩子
            hook = User32.SetWindowsHookEx(User32.HookType.WH_KEYBOARD_LL, hookProc, Kernel32.GetModuleHandle(Process.GetCurrentProcess().MainModule!.ModuleName), 0);
            
            // 安装线程局部键盘钩子
            User32.SetWindowsHookEx(User32.HookType.WH_KEYBOARD_LL, hookProc, IntPtr.Zero, (int)Kernel32.GetCurrentThreadId());

            if (hook.IsNull)  // 检查是否成功安装钩子
            {
                Stop();  // 停止钩子
                throw new SystemException("Failed to install keyboard hook");  // 抛出异常表示安装失败
            }
        }
    }

    public void Stop()  // 停止钩子的设置
    {
        bool retKeyboard = true;  // 初始化返回值变量，默认值为 true

        if (!hook.IsNull)  // 检查钩子是否为空
        {
            retKeyboard = User32.UnhookWindowsHookEx(hook);  // 卸载钩子，并记录返回值
            hook = new(IntPtr.Zero);  // 重新初始化钩子句柄为 IntPtr.Zero
        }

        if (!retKeyboard)  // 检查卸载钩子的操作是否成功
        {
            throw new SystemException("Failed to uninstall hook");  // 抛出异常表示卸载失败
        }
    }

    private nint KeyboardHookProc(int nCode, nint wParam, nint lParam)  // 键盘钩子的回调函数
    {
        # 检查 nCode 是否大于或等于 0，通常表示处理正常的钩子事件
        if (nCode >= 0)
        {
            # 检查是否有 KeyDown、KeyUp 或 KeyPress 事件处理程序
            if (KeyDown != null || KeyUp != null || KeyPress != null)
            {
                # 从 lParam 参数中获取 KBDLLHOOKSTRUCT 结构体
                User32.KBDLLHOOKSTRUCT hookStruct = (User32.KBDLLHOOKSTRUCT)Marshal.PtrToStructure(lParam, typeof(User32.KBDLLHOOKSTRUCT))!;

                # 如果有 KeyDown 处理程序，并且 wParam 表示键盘按下或系统键盘按下消息
                if (KeyDown != null && (wParam == (nint)User32.WindowMessage.WM_KEYDOWN || wParam == (nint)User32.WindowMessage.WM_SYSKEYDOWN))
                {
                    # 获取按键的虚拟键码
                    KeyboardKeys keyData = (KeyboardKeys)hookStruct.vkCode;
                    # 创建 KeyboardEventArgs 对象
                    KeyboardEventArgs e = new(keyData);
                    # 调用 KeyDown 事件处理程序
                    KeyDown(this, e);
                }

                # 如果有 KeyPress 处理程序，并且 wParam 表示键盘按下消息
                if (KeyPress != null && wParam == (nint)User32.WindowMessage.WM_KEYDOWN)
                {
                    # 创建一个字节数组来存储键盘状态
                    byte[] keyState = new byte[256];
                    # 获取当前键盘状态
                    _ = User32.GetKeyboardState(keyState);

                    # 将虚拟键码转换为 ASCII 字符
                    if (User32.ToAscii(hookStruct.vkCode, hookStruct.scanCode, keyState, out ushort lpChar, hookStruct.flags) == 1)
                    {
                        # 创建 KeyboardPressEventArgs 对象
                        KeyboardPressEventArgs e = new((char)lpChar);
                        # 调用 KeyPress 事件处理程序
                        KeyPress(this, e);
                    }
                }

                # 如果有 KeyUp 处理程序，并且 wParam 表示键盘抬起或系统键盘抬起消息
                if (KeyUp != null && (wParam == (nint)User32.WindowMessage.WM_KEYUP || wParam == (nint)User32.WindowMessage.WM_SYSKEYUP))
                {
                    # 获取按键的虚拟键码
                    KeyboardKeys keyData = (KeyboardKeys)hookStruct.vkCode;
                    # 创建 KeyboardEventArgs 对象
                    KeyboardEventArgs e = new(keyData);
                    # 调用 KeyUp 事件处理程序
                    KeyUp(this, e);
                }
            }
        }
        # 传递钩子事件到下一个钩子
        return User32.CallNextHookEx(hook, nCode, wParam, lParam);
    }
请提供需要注释的代码，我会帮你添加解释。
```