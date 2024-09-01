# `.\better-genshin-impact\Fischless.WindowsInput\IInputSimulator.cs`

```cs
# 定义一个名为 IInputSimulator 的接口
﻿namespace Fischless.WindowsInput;

public interface IInputSimulator
{
    # 定义一个只读属性 Keyboard，类型为 IKeyboardSimulator
    public IKeyboardSimulator Keyboard { get; }
    # 定义一个只读属性 Mouse，类型为 IMouseSimulator
    public IMouseSimulator Mouse { get; }
    # 定义一个只读属性 InputDeviceState，类型为 IInputDeviceStateAdaptor
    public IInputDeviceStateAdaptor InputDeviceState { get; }
}
```