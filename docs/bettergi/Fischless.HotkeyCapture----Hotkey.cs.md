# `.\better-genshin-impact\Fischless.HotkeyCapture\Hotkey.cs`

```cs
﻿using Vanara.PInvoke;  // 引入 Vanara.PInvoke 命名空间

namespace Fischless.HotkeyCapture;  // 定义命名空间

public sealed class Hotkey  // 定义一个密封类 Hotkey
{
    public bool Alt { get; set; }  // 属性：是否按下 Alt 键
    public bool Control { get; set; }  // 属性：是否按下 Control 键
    public bool Shift { get; set; }  // 属性：是否按下 Shift 键
    public bool Windows { get; set; }  // 属性：是否按下 Windows 键

    private Keys key;  // 私有字段：保存按下的键

    public Keys Key  // 属性：获取或设置按下的键
    {
        get => key;  // 获取当前键值
        set  // 设置键值
        {
            // 如果设置的键不是控制键、Alt 键、菜单键或 Shift 键，则更新字段
            if (value != Keys.ControlKey && value != Keys.Alt && value != Keys.Menu && value != Keys.ShiftKey)
            {
                key = value;
            }
            else  // 否则，设置为无键
            {
                key = Keys.None;
            }
        }
    }

    public User32.HotKeyModifiers ModifierKey =>  // 属性：获取修饰键的位掩码
        (Windows ? User32.HotKeyModifiers.MOD_WIN : User32.HotKeyModifiers.MOD_NONE) |  // 判断是否有 Windows 键
        (Control ? User32.HotKeyModifiers.MOD_CONTROL : User32.HotKeyModifiers.MOD_NONE) |  // 判断是否有 Control 键
        (Shift ? User32.HotKeyModifiers.MOD_SHIFT : User32.HotKeyModifiers.MOD_NONE) |  // 判断是否有 Shift 键
        (Alt ? User32.HotKeyModifiers.MOD_ALT : User32.HotKeyModifiers.MOD_NONE);  // 判断是否有 Alt 键

    public Hotkey()  // 构造函数：初始化 Hotkey 对象
    {
        Reset();  // 调用 Reset 方法
    }

    public Hotkey(string hotkeyStr)  // 构造函数：根据字符串初始化 Hotkey 对象
    {
        try
        {
            string[] keyStrs = hotkeyStr.Replace(" ", string.Empty).Split('+');  // 处理热键字符串，去除空格并分割

            foreach (string keyStr in keyStrs)  // 遍历每个键字符串
            {
                if (keyStr.Equals("Win", StringComparison.OrdinalIgnoreCase))  // 如果是 Windows 键
                {
                    Windows = true;
                }
                else if (keyStr.Equals("Ctrl", StringComparison.OrdinalIgnoreCase))  // 如果是 Control 键
                {
                    Control = true;
                }
                else if (keyStr.Equals("Shift", StringComparison.OrdinalIgnoreCase))  // 如果是 Shift 键
                {
                    Shift = true;
                }
                else if (keyStr.Equals("Alt", StringComparison.OrdinalIgnoreCase))  // 如果是 Alt 键
                {
                    Alt = true;
                }
                else  // 否则，解析为具体的按键
                {
                    Key = (Keys)Enum.Parse(typeof(Keys), keyStr);
                }
            }
        }
        catch  // 捕捉异常
        {
            throw new ArgumentException("Invalid Hotkey");  // 抛出参数异常
        }
    }

    public override string ToString()  // 重写 ToString 方法
    {
        string str = string.Empty;  // 初始化字符串
        if (Key != Keys.None)  // 如果键不是无键
        {
            str = string.Format("{0}{1}{2}{3}{4}",  // 格式化输出字符串
                Windows ? "Win + " : string.Empty,  // 如果有 Windows 键，添加 "Win + "
                Control ? "Ctrl + " : string.Empty,  // 如果有 Control 键，添加 "Ctrl + "
                Shift ? "Shift + " : string.Empty,  // 如果有 Shift 键，添加 "Shift + "
                Alt ? "Alt + " : string.Empty,  // 如果有 Alt 键，添加 "Alt + "
                Key);  // 添加具体的按键
        }
        return str;  // 返回格式化字符串
    }

    public void Reset()  // 方法：重置 Hotkey 对象
    {
        Alt = false;  // 取消 Alt 键
        Control = false;  // 取消 Control 键
        Shift = false;  // 取消 Shift 键
        Windows = false;  // 取消 Windows 键
        Key = Keys.None;  // 设置按键为无
    }
}
```