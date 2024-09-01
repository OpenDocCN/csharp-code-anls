# `.\better-genshin-impact\Fischless.KeyboardCapture\KeyboardResult.cs`

```cs
﻿namespace Fischless.KeyboardCapture;  // 定义一个名为 Fischless.KeyboardCapture 的命名空间

public sealed class KeyboardResult  // 定义一个不可继承的类 KeyboardResult
{
    public KeyboardItem KeyboardItem { get; init; } = default;  // 自动属性，存储键盘项，初始化为默认值
    public string Key => KeyboardItem.Key ?? KeyboardItem.KeyCode.ToName();  // 只读属性，获取键盘项的键名，如果为 null，则调用 KeyCode.ToName() 方法
    public bool IsShift { get; set; } = false;  // 自动属性，指示是否按下 Shift 键，默认值为 false
    public bool IsCtrl { get; set; } = false;  // 自动属性，指示是否按下 Ctrl 键，默认值为 false
    public bool IsAlt { get; set; } = false;  // 自动属性，指示是否按下 Alt 键，默认值为 false
    public bool IsWin { get; set; } = false;  // 自动属性，指示是否按下 Win 键，默认值为 false
    public bool IsExtendedKey { get; set; } = false;  // 自动属性，指示是否按下扩展键，默认值为 false

    public KeyboardResult(KeyboardItem keyboardItem)  // 构造函数，初始化 KeyboardResult 类的实例
    {
        KeyboardItem = keyboardItem;  // 将传入的 KeyboardItem 赋值给 KeyboardItem 属性
    }

    public override string ToString()  // 重写 ToString 方法，用于将对象转换为字符串
    {
        List<string> keyModifiers = [];  // 初始化一个空的字符串列表，用于存储键盘修饰符

        if (IsCtrl)  // 如果 Ctrl 键被按下
        {
            return KeyboardConst.Ctrl;  // 返回 Ctrl 的常量表示
        }

        if (IsShift)  // 如果 Shift 键被按下
        {
            return KeyboardConst.Shift;  // 返回 Shift 的常量表示
        }

        if (IsAlt)  // 如果 Alt 键被按下
        {
            return KeyboardConst.Alt;  // 返回 Alt 的常量表示
        }

        if (IsWin)  // 如果 Win 键被按下
        {
            return KeyboardConst.Win;  // 返回 Win 的常量表示
        }

        if (keyModifiers.Count > 0)  // 如果有键盘修饰符
        {
            if (KeyboardItem.KeyCode.IsCombinationKey())  // 如果键码表示为组合键
            {
                if (IsCtrl)  // 如果 Ctrl 键被按下
                {
                    return KeyboardConst.Ctrl;  // 返回 Ctrl 的常量表示
                }

                if (IsShift)  // 如果 Shift 键被按下
                {
                    return KeyboardConst.Shift;  // 返回 Shift 的常量表示
                }

                if (IsAlt)  // 如果 Alt 键被按下
                {
                    return KeyboardConst.Alt;  // 返回 Alt 的常量表示
                }

                if (IsWin)  // 如果 Win 键被按下
                {
                    return KeyboardConst.Win;  // 返回 Win 的常量表示
                }
            }
            else  // 如果不是组合键
            {
                return $"{string.Join('+', keyModifiers)}+{Key}";  // 返回修饰符和键名的组合
            }
        }
        return Key;  // 如果没有修饰符，返回键名
    }

    public static implicit operator string(KeyboardResult result) => result.ToString();  // 隐式转换运算符，将 KeyboardResult 转换为字符串
}
```