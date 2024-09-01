# `.\better-genshin-impact\Fischless.WindowsInput\InputSimulator.cs`

```cs
# 定义命名空间 Fischless.WindowsInput
﻿namespace Fischless.WindowsInput;

# 定义一个名为 InputSimulator 的公共类，实现 IInputSimulator 接口
public class InputSimulator : IInputSimulator
{
    # 类的构造函数，接受键盘、鼠标和输入设备状态适配器作为参数
    public InputSimulator(IKeyboardSimulator keyboardSimulator, IMouseSimulator mouseSimulator, IInputDeviceStateAdaptor inputDeviceStateAdaptor)
    {
        # 初始化私有字段 _keyboardSimulator
        _keyboardSimulator = keyboardSimulator;
        # 初始化私有字段 _mouseSimulator
        _mouseSimulator = mouseSimulator;
        # 初始化私有字段 _inputDeviceState
        _inputDeviceState = inputDeviceStateAdaptor;
    }

    # 默认构造函数，无参数
    public InputSimulator()
    {
        # 创建一个新的 KeyboardSimulator 实例并赋值给 _keyboardSimulator
        _keyboardSimulator = new KeyboardSimulator(this);
        # 创建一个新的 MouseSimulator 实例并赋值给 _mouseSimulator
        _mouseSimulator = new MouseSimulator(this);
        # 创建一个新的 WindowsInputDeviceStateAdaptor 实例并赋值给 _inputDeviceState
        _inputDeviceState = new WindowsInputDeviceStateAdaptor();
    }

    # 属性获取器，返回键盘模拟器
    public IKeyboardSimulator Keyboard => _keyboardSimulator;

    # 属性获取器，返回鼠标模拟器
    public IMouseSimulator Mouse => _mouseSimulator;

    # 属性获取器，返回输入设备状态适配器
    public IInputDeviceStateAdaptor InputDeviceState => _inputDeviceState;

    # 定义私有的只读字段 _keyboardSimulator，用于存储键盘模拟器的实例
    private readonly IKeyboardSimulator _keyboardSimulator;

    # 定义私有的只读字段 _mouseSimulator，用于存储鼠标模拟器的实例
    private readonly IMouseSimulator _mouseSimulator;

    # 定义私有的只读字段 _inputDeviceState，用于存储输入设备状态适配器的实例
    private readonly IInputDeviceStateAdaptor _inputDeviceState;
}
```