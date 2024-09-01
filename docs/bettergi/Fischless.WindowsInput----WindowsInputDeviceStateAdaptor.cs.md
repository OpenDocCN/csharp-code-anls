# `.\better-genshin-impact\Fischless.WindowsInput\WindowsInputDeviceStateAdaptor.cs`

```cs
# 引入 Vanara.PInvoke 命名空间以便使用 Windows API
﻿using Vanara.PInvoke;

# 定义一个命名空间，组织相关类
namespace Fischless.WindowsInput;

# 创建一个类 WindowsInputDeviceStateAdaptor，实现 IInputDeviceStateAdaptor 接口
public class WindowsInputDeviceStateAdaptor : IInputDeviceStateAdaptor
{
    # 定义方法 IsKeyDown，判断给定的键是否被按下
    public bool IsKeyDown(User32.VK keyCode)
    {
        # 使用 GetKeyState 方法获取指定键的状态，并将结果存储在 keyState 变量
        short keyState = User32.GetKeyState((ushort)keyCode);
        # 如果 keyState 小于 0，表示键被按下，返回 true，否则返回 false
        return keyState < 0;
    }

    # 定义方法 IsKeyUp，判断给定的键是否已经松开
    public bool IsKeyUp(User32.VK keyCode)
    {
        # 通过取反 IsKeyDown 方法的结果来判断键是否松开
        return !IsKeyDown(keyCode);
    }

    # 定义方法 IsHardwareKeyDown，判断给定的硬件键是否被按下
    public bool IsHardwareKeyDown(User32.VK keyCode)
    {
        # 使用 GetAsyncKeyState 方法获取指定键的异步状态，并存储在 asyncKeyState 变量
        short asyncKeyState = User32.GetAsyncKeyState((ushort)keyCode);
        # 如果 asyncKeyState 小于 0，表示硬件键被按下，返回 true，否则返回 false
        return asyncKeyState < 0;
    }

    # 定义方法 IsHardwareKeyUp，判断给定的硬件键是否已经松开
    public bool IsHardwareKeyUp(User32.VK keyCode)
    {
        # 通过取反 IsHardwareKeyDown 方法的结果来判断硬件键是否松开
        return !IsHardwareKeyDown(keyCode);
    }

    # 定义方法 IsTogglingKeyInEffect，判断切换键（如 Caps Lock）是否处于激活状态
    public bool IsTogglingKeyInEffect(User32.VK keyCode)
    {
        # 使用 GetKeyState 方法获取指定键的状态，并存储在 keyState 变量
        short keyState = User32.GetKeyState((ushort)keyCode);
        # 检查 keyState 的最低有效位是否为 1，表示切换键处于激活状态
        return (keyState & 1) == 1;
    }
}
```