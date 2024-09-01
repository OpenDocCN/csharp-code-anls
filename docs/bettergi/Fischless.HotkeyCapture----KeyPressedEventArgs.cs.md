# `.\better-genshin-impact\Fischless.HotkeyCapture\KeyPressedEventArgs.cs`

```cs
# 引入 Vanara.PInvoke 命名空间以便使用 Win32 API 函数
﻿using Vanara.PInvoke;

# 声明命名空间 Fischless.HotkeyCapture
namespace Fischless.HotkeyCapture;

# 定义一个继承自 EventArgs 的类 KeyPressedEventArgs
public class KeyPressedEventArgs : EventArgs
{
    # 定义只读属性 Modifier，表示按键的修饰符（如 Ctrl、Alt 等）
    public User32.HotKeyModifiers Modifier { get; }
    # 定义只读属性 Key，表示被按下的键
    public Keys Key { get; }

    # 构造函数，用于初始化 KeyPressedEventArgs 类的实例
    public KeyPressedEventArgs(User32.HotKeyModifiers modifier, Keys key)
    {
        # 设置 Modifier 属性的值
        Modifier = modifier;
        # 设置 Key 属性的值
        Key = key;
    }
}
```