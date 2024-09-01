# `.\better-genshin-impact\BetterGenshinImpact\Model\HotKey.cs`

```cs
# 引入必要的命名空间
﻿using System;
using System.Text;
using System.Windows.Input;

# 定义一个不可变的部分记录结构体 HotKey，包含键、修饰键和鼠标按钮
namespace BetterGenshinImpact.Model;

public readonly partial record struct HotKey(Key Key, ModifierKeys Modifiers = ModifierKeys.None, MouseButton MouseButton = MouseButton.Left)
{
    # 重写 ToString 方法，将 HotKey 对象转换为字符串表示
    public override string ToString()
    {
        # 如果所有字段均为默认值，返回 "< None >"
        if (Key == Key.None && Modifiers == ModifierKeys.None && MouseButton == MouseButton.Left)
        {
            return "< None >";
        }

        # 如果鼠标按钮不是左键，直接返回鼠标按钮的字符串表示
        if (MouseButton != MouseButton.Left)
        {
            return MouseButton.ToString();
        }

        # 创建一个 StringBuilder 用于构建最终的字符串
        var buffer = new StringBuilder();

        # 检查并附加修饰键
        if (Modifiers.HasFlag(ModifierKeys.Control))
            buffer.Append("Ctrl + ");
        if (Modifiers.HasFlag(ModifierKeys.Shift))
            buffer.Append("Shift + ");
        if (Modifiers.HasFlag(ModifierKeys.Alt))
            buffer.Append("Alt + ");
        if (Modifiers.HasFlag(ModifierKeys.Windows))
            buffer.Append("Win + ");

        # 附加键的字符串表示
        buffer.Append(Key);

        # 返回构建的字符串
        return buffer.ToString();
    }

    # 判断 HotKey 对象是否为空，即所有字段均为默认值
    public bool IsEmpty => Key == Key.None && Modifiers == ModifierKeys.None && MouseButton == MouseButton.Left;

    # 从字符串创建一个 HotKey 对象
    public static HotKey FromString(string str)
    {
        # 初始化默认值
        var key = Key.None;
        var modifiers = ModifierKeys.None;
        var mouseButton = MouseButton.Left;
        
        # 如果字符串为空或为 "< None >"，返回默认 HotKey
        if (string.IsNullOrWhiteSpace(str) || string.Equals(str, "< None >"))
        {
            return new HotKey(key, modifiers, mouseButton);
        }

        # 如果字符串表示为鼠标 XButton1，返回相应的 HotKey
        if (str == MouseButton.XButton1.ToString())
        {
            return new HotKey(key, modifiers, MouseButton.XButton1);
        }

        # 如果字符串表示为鼠标 XButton2，返回相应的 HotKey
        if (str == MouseButton.XButton2.ToString())
        {
            return new HotKey(key, modifiers, MouseButton.XButton2);
        }

        # 按 "+" 拆分字符串为各部分
        var parts = str.Split('+');
        foreach (var part in parts)
        {
            # 去除各部分的前后空格
            var trimmed = part.Trim();
            # 根据每部分更新修饰键或鼠标按钮
            if (trimmed == "Ctrl")
                modifiers |= ModifierKeys.Control;
            else if (trimmed == "Shift")
                modifiers |= ModifierKeys.Shift;
            else if (trimmed == "Alt")
                modifiers |= ModifierKeys.Alt;
            else if (trimmed == "Win")
                modifiers |= ModifierKeys.Windows;
            else if (trimmed == "XButton1")
                mouseButton = MouseButton.XButton1;
            else if (trimmed == "XButton2")
                mouseButton = MouseButton.XButton2;
            else
                key = (Key)Enum.Parse(typeof(Key), trimmed);
        }

        # 返回构建的 HotKey 对象
        return new HotKey(key, modifiers, mouseButton);
    }

    # 判断给定的名称是否为鼠标按钮
    public static bool IsMouseButton(string name)
    {
        return name == MouseButton.XButton1.ToString() || name == MouseButton.XButton2.ToString();
    }
}

# 定义一个额外的部分记录结构体 HotKey，包含一个静态属性 None
public partial record struct HotKey
{
    # 返回一个表示没有热键的默认 HotKey 对象
    public static HotKey None { get; } = new();
}
```