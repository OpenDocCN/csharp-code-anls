# `.\better-genshin-impact\Fischless.HotkeyCapture\HotkeyHolder.cs`

```cs
# 定义一个不可继承的类 HotkeyHolder，用于管理热键
public sealed class HotkeyHolder
{
    # 定义静态字段 hotkey 用于存储当前热键
    private static Hotkey? hotkey;
    # 定义静态字段 hotkeyHook 用于处理热键钩子
    private static HotkeyHook? hotkeyHook;
    # 定义静态字段 keyPressed 用于存储按键按下时的回调
    private static Action<object?, KeyPressedEventArgs>? keyPressed;

    # 注册热键的方法，接受热键字符串和可选的按键按下回调
    public static void RegisterHotKey(string hotkeyStr, Action<object?, KeyPressedEventArgs> keyPressed = null!)
    {
        # 如果热键字符串为空或为 null，注销热键并返回
        if (string.IsNullOrEmpty(hotkeyStr))
        {
            UnregisterHotKey();
            return;
        }

        # 创建一个新的 Hotkey 实例
        hotkey = new Hotkey(hotkeyStr);

        # 释放之前的热键钩子资源
        hotkeyHook?.Dispose();
        # 创建新的热键钩子实例
        hotkeyHook = new HotkeyHook();
        # 解除旧的按键按下事件处理程序
        hotkeyHook.KeyPressed -= OnKeyPressed;
        # 添加新的按键按下事件处理程序
        hotkeyHook.KeyPressed += OnKeyPressed;
        # 设置按键按下回调
        HotkeyHolder.keyPressed = keyPressed;
        # 注册新的热键
        hotkeyHook.RegisterHotKey(hotkey.ModifierKey, hotkey.Key);
    }

    # 注销热键的方法
    public static void UnregisterHotKey()
    {
        # 如果热键钩子存在
        if (hotkeyHook != null)
        {
            # 解除按键按下事件处理程序
            hotkeyHook.KeyPressed -= OnKeyPressed;
            # 注销热键
            hotkeyHook.UnregisterHotKey();
            # 释放热键钩子资源
            hotkeyHook.Dispose();
        }
    }

    # 按键按下事件处理程序
    private static void OnKeyPressed(object? sender, KeyPressedEventArgs e)
    {
        # 如果有设置按键按下回调，则调用它
        keyPressed?.Invoke(sender, e);
    }
}
```