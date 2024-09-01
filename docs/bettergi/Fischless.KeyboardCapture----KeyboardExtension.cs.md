# `.\better-genshin-impact\Fischless.KeyboardCapture\KeyboardExtension.cs`

```cs
﻿using Vanara.PInvoke;

namespace Fischless.KeyboardCapture;

public static class KeyboardExtension
{
    // 检查指定的键是否被锁定（例如 Caps Lock、Num Lock 等）
    public static bool IsKeyLocked(this KeyboardKeys keyVal)
    {
        // 确保该键是一个锁定键
        if (keyVal == KeyboardKeys.Insert || keyVal == KeyboardKeys.NumLock || keyVal == KeyboardKeys.CapsLock || keyVal == KeyboardKeys.Scroll)
        {
            // 获取键的状态，返回结果包含高低位信息
            int result = User32.GetKeyState((int)keyVal);

            // 如果高位是1，则键被按下；否则，键未被按下
            // 如果低位是1，则键被切换。比如 Caps Lock 键，如果被打开则为切换状态
            // 如果低位是0，则键未切换。指示灯（如果有）在键被切换时亮起
            // 在切换键中（只关注低位）
            if (keyVal == KeyboardKeys.Insert || keyVal == KeyboardKeys.CapsLock)
            {
                // 仅检查低位以确定键是否被切换
                return (result & 0x1) != 0x0;
            }

            // 对于其他锁定键（如 Num Lock 和 Scroll Lock），检查高低位
            return (result & 0x8001) != 0x0;
        }

        // 如果键不是锁定键，则抛出不支持异常
        throw new NotSupportedException("Specified key is not supported.");
    }

    // 检查指定的键是否是扩展键（例如 Enter 键）
    public static bool IsExtendedKey(this KeyboardKeys keyVal)
    {
        switch (keyVal)
        {
            case KeyboardKeys.Return:
                // 获取 Enter 键的状态
                short state = User32.GetKeyState((int)KeyboardKeys.Return);
                // 判断是否为扩展键的特定状态
                return state == 0x01 || state == -128;
        }
        // 默认返回 false，表示不是扩展键
        return false;
    }

    // 检查指定的键是否为组合键（如 Shift、Ctrl、Alt 等）
    public static bool IsCombinationKey(this KeyboardKeys keyVal)
    {
        return keyVal switch
        {
            // 定义组合键的集合
            KeyboardKeys.Shift
            or KeyboardKeys.ShiftKey
            or KeyboardKeys.LShiftKey
            or KeyboardKeys.RShiftKey
            or KeyboardKeys.Control
            or KeyboardKeys.ControlKey
            or KeyboardKeys.LControlKey
            or KeyboardKeys.RControlKey
            or KeyboardKeys.LWin
            or KeyboardKeys.RWin
            or KeyboardKeys.Alt
            or KeyboardKeys.LMenu
            or KeyboardKeys.RMenu => true,
            _ => false,
        };
    }

    // 转换键值为名称（此方法尚未实现）
    public static string ToName(this KeyboardKeys keyVal, bool useStrict = false)
    }
}

public enum KeyboardNameType
{
    Default,
    Strict,
}
```