# `.\better-genshin-impact\BetterGenshinImpact\Model\KeyboardHook.cs`

```cs
﻿using BetterGenshinImpact.GameTask; // 引入 BetterGenshinImpact.GameTask 命名空间
using Fischless.HotkeyCapture; // 引入 Fischless.HotkeyCapture 命名空间
using System; // 引入 System 命名空间
using System.Collections.Generic; // 引入 System.Collections.Generic 命名空间
using System.Threading.Tasks; // 引入 System.Threading.Tasks 命名空间
using System.Windows.Forms; // 引入 System.Windows.Forms 命名空间
using Vanara.PInvoke; // 引入 Vanara.PInvoke 命名空间

namespace BetterGenshinImpact.Model; // 定义 BetterGenshinImpact.Model 命名空间

public class KeyboardHook // 定义 KeyboardHook 类
{
    // 定义一个静态字典，存储所有键盘钩子的实例，键为 Keys 类型，值为 KeyboardHook 类型
    public static Dictionary<Keys, KeyboardHook> AllKeyboardHooks = [];

    // 定义 KeyPressedEvent 事件，当按键被按下时触发
    public event EventHandler<KeyPressedEventArgs>? KeyPressedEvent = null;

    // 定义 KeyDownEvent 事件，当按键被按下时触发
    public event EventHandler<KeyPressedEventArgs>? KeyDownEvent = null;

    // 定义 KeyUpEvent 事件，当按键被释放时触发
    public event EventHandler<KeyPressedEventArgs>? KeyUpEvent = null;

    // 属性，指示是否需要长按时持续触发事件
    public bool IsHold { get; set; }

    // 属性，绑定的键
    public Keys BindKey { get; set; } = Keys.None;

    // 属性，指示按键当前是否被按下
    public bool IsPressed { get; set; }

    /// <summary>
    /// 注意长按的时候会一直触发KeyDown
    /// </summary>
    /// <param name="sender"></param>
    /// <param name="e"></param>
    public void KeyDown(object? sender, KeyEventArgs e) // 处理按键按下事件
    {
        // 如果当前 GenshinImpact 没有激活，则返回
        if (!SystemControl.IsGenshinImpactActive())
        {
            return;
        }

        // 如果按下的键与绑定键相同
        if (e.KeyCode == BindKey)
        {
            IsPressed = true; // 设置按键状态为按下
            // 触发 KeyDownEvent 事件
            KeyDownEvent?.Invoke(this, new KeyPressedEventArgs(User32.HotKeyModifiers.MOD_NONE, e.KeyCode));
            // 如果需要长按处理
            if (IsHold)
            {
                if (KeyPressedEvent != null)
                {
                    // 异步运行 RunAction 方法，处理持续按键
                    Task.Run(() => RunAction(e));
                }
            }
            else
            {
                // 如果不需要长按，触发 KeyPressedEvent 事件并重置按键状态
                KeyPressedEvent?.Invoke(this, new KeyPressedEventArgs(User32.HotKeyModifiers.MOD_NONE, e.KeyCode));
                IsPressed = false;
            }
        }
    }

    /// <summary>
    /// 长按持续执行
    /// </summary>
    /// <param name="e"></param>
    private void RunAction(KeyEventArgs e) // 持续触发 KeyPressedEvent 事件的方法
    {
        lock (this) // 锁定当前实例以确保线程安全
        {
            while (IsPressed && KeyPressedEvent != null) // 当按键状态为按下并且 KeyPressedEvent 不为 null
            {
                // 触发 KeyPressedEvent 事件
                KeyPressedEvent?.Invoke(this, new KeyPressedEventArgs(User32.HotKeyModifiers.MOD_NONE, e.KeyCode));
            }
        }
    }

    public void KeyUp(object? sender, KeyEventArgs e) // 处理按键释放事件
    {
        // 如果释放的键与绑定键相同
        if (e.KeyCode == BindKey)
        {
            IsPressed = false; // 设置按键状态为未按下
            // 如果当前 GenshinImpact 激活，触发 KeyUpEvent 事件
            if (SystemControl.IsGenshinImpactActive())
            {
                KeyUpEvent?.Invoke(this, new KeyPressedEventArgs(User32.HotKeyModifiers.MOD_NONE, e.KeyCode));
            }
        }
    }

    public void RegisterHotKey(Keys key) // 注册一个热键
    {
        BindKey = key; // 设置绑定键
        AllKeyboardHooks.Add(key, this); // 将当前实例添加到字典中
    }

    public void UnregisterHotKey() // 注销热键
    {
        IsPressed = false; // 设置按键状态为未按下
        IsHold = false; // 重置长按标志
        AllKeyboardHooks.Remove(BindKey); // 从字典中移除当前实例
    }

    public void Dispose() // 释放资源
    {
        UnregisterHotKey(); // 注销热键
    }
}
```