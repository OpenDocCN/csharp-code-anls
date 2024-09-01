# `.\better-genshin-impact\BetterGenshinImpact\Model\MouseHook.cs`

```cs
﻿using BetterGenshinImpact.GameTask;  // 引入 BetterGenshinImpact.GameTask 命名空间
using Fischless.HotkeyCapture;  // 引入 Fischless.HotkeyCapture 命名空间
using Gma.System.MouseKeyHook;  // 引入 Gma.System.MouseKeyHook 命名空间
using System;  // 引入 System 命名空间
using System.Collections.Generic;  // 引入 System.Collections.Generic 命名空间
using System.Threading.Tasks;  // 引入 System.Threading.Tasks 命名空间
using System.Windows.Forms;  // 引入 System.Windows.Forms 命名空间
using Vanara.PInvoke;  // 引入 Vanara.PInvoke 命名空间

namespace BetterGenshinImpact.Model;  // 定义 BetterGenshinImpact.Model 命名空间

public class MouseHook  // 定义 MouseHook 类
{
    // 存储所有 MouseHook 实例的字典，按 MouseButtons 索引
    public static Dictionary<MouseButtons, MouseHook> AllMouseHooks = [];

    // 鼠标按下事件
    public event EventHandler<KeyPressedEventArgs>? MousePressed = null;

    // 鼠标按下事件
    public event EventHandler<KeyPressedEventArgs>? MouseDownEvent = null;

    // 鼠标释放事件
    public event EventHandler<KeyPressedEventArgs>? MouseUpEvent = null;

    // 是否持续按住鼠标按钮
    public bool IsHold { get; set; }

    // 当前绑定的鼠标按钮，默认为左键
    public MouseButtons BindMouse { get; set; } = MouseButtons.Left;

    // 当前鼠标按钮是否被按下
    public bool IsPressed { get; set; }

    // 处理鼠标按下事件
    public void MouseDown(object? sender, MouseEventExtArgs e)
    {
        // 如果 Genshin Impact 未激活，直接返回
        if (!SystemControl.IsGenshinImpactActive())
        {
            return;
        }

        // 如果当前按钮是绑定的鼠标按钮
        if (e.Button != MouseButtons.Left && e.Button != MouseButtons.None && e.Button == BindMouse)
        {
            IsPressed = true;  // 设置按钮按下状态为 true
            // 触发 MouseDownEvent 事件
            MouseDownEvent?.Invoke(this, new KeyPressedEventArgs(User32.HotKeyModifiers.MOD_NONE, Keys.None));
            // 如果 IsHold 为 true，异步执行 RunAction 方法
            if (IsHold)
            {
                Task.Run(() => RunAction(e));
            }
            else
            {
                // 否则，触发 MousePressed 事件，并将 IsPressed 设置为 false
                MousePressed?.Invoke(this, new KeyPressedEventArgs(User32.HotKeyModifiers.MOD_NONE, Keys.None));
                IsPressed = false;
            }
        }
    }

    /// <summary>
    /// 长按持续执行
    /// </summary>
    /// <param name="e"></param>
    private void RunAction(MouseEventExtArgs e)
    {
        lock (this)  // 确保线程安全
        {
            // 当 IsPressed 为 true 时持续触发 MousePressed 事件
            while (IsPressed)
            {
                MousePressed?.Invoke(this, new KeyPressedEventArgs(User32.HotKeyModifiers.MOD_NONE, Keys.None));
            }
        }
    }

    // 处理鼠标释放事件
    public void MouseUp(object? sender, MouseEventExtArgs e)
    {
        // 如果当前按钮是绑定的鼠标按钮
        if (e.Button != MouseButtons.Left && e.Button != MouseButtons.None && e.Button == BindMouse)
        {
            IsPressed = false;  // 设置按钮按下状态为 false
            // 如果 Genshin Impact 仍然激活，触发 MouseUpEvent 事件
            if (SystemControl.IsGenshinImpactActive())
            {
                MouseUpEvent?.Invoke(this, new KeyPressedEventArgs(User32.HotKeyModifiers.MOD_NONE, Keys.None));
            }
        }
    }

    // 注册热键
    public void RegisterHotKey(MouseButtons mouseButton)
    {
        BindMouse = mouseButton;  // 设置绑定的鼠标按钮
        AllMouseHooks.Add(mouseButton, this);  // 将当前 MouseHook 实例添加到 AllMouseHooks 字典
    }

    // 注销热键
    public void UnregisterHotKey()
    {
        IsPressed = false;  // 设置按钮按下状态为 false
        IsHold = false;  // 设置是否持续按住为 false
        AllMouseHooks.Remove(BindMouse);  // 从 AllMouseHooks 字典中移除当前 MouseHook 实例
    }

    // 释放资源
    public void Dispose()
    {
        UnregisterHotKey();  // 注销热键
    }
}
```