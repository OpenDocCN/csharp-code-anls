# `.\better-genshin-impact\Fischless.WindowsInput\IKeyboardSimulator.cs`

```cs
# 引用 Vanara.PInvoke 库中的内容
﻿using Vanara.PInvoke;

# 定义一个命名空间 Fischless.WindowsInput
namespace Fischless.WindowsInput;

# 定义一个接口 IKeyboardSimulator
public interface IKeyboardSimulator
{
    # 定义一个只读属性 Mouse，返回 IMouseSimulator 类型
    public IMouseSimulator Mouse { get; }

    # 定义一个方法 KeyDown，用于按下指定的键
    public IKeyboardSimulator KeyDown(User32.VK keyCode);

    # 定义一个方法 KeyDown，按下指定的键，可以选择是否为扩展键
    public IKeyboardSimulator KeyDown(bool? isExtendedKey, User32.VK keyCode);

    # 定义一个方法 KeyPress，用于按下并释放指定的键
    public IKeyboardSimulator KeyPress(User32.VK keyCode);

    # 定义一个方法 KeyPress，按下并释放指定的键，可以选择是否为扩展键
    public IKeyboardSimulator KeyPress(bool? isExtendedKey, User32.VK keyCode);

    # 定义一个方法 KeyPress，用于同时按下和释放多个键
    public IKeyboardSimulator KeyPress(params User32.VK[] keyCodes);

    # 定义一个方法 KeyPress，按下和释放多个键，可以选择是否为扩展键
    public IKeyboardSimulator KeyPress(bool? isExtendedKey, params User32.VK[] keyCodes);

    # 定义一个方法 KeyUp，用于释放指定的键
    public IKeyboardSimulator KeyUp(User32.VK keyCode);

    # 定义一个方法 KeyUp，释放指定的键，可以选择是否为扩展键
    public IKeyboardSimulator KeyUp(bool? isExtendedKey, User32.VK keyCode);

    # 定义一个方法 ModifiedKeyStroke，进行修饰键与普通键组合的操作
    public IKeyboardSimulator ModifiedKeyStroke(IEnumerable<User32.VK> modifierKeyCodes, IEnumerable<User32.VK> keyCodes);

    # 定义一个方法 ModifiedKeyStroke，进行修饰键与单个键组合的操作
    public IKeyboardSimulator ModifiedKeyStroke(IEnumerable<User32.VK> modifierKeyCodes, User32.VK keyCode);

    # 定义一个方法 ModifiedKeyStroke，进行单个修饰键与多个普通键组合的操作
    public IKeyboardSimulator ModifiedKeyStroke(User32.VK modifierKey, IEnumerable<User32.VK> keyCodes);

    # 定义一个方法 ModifiedKeyStroke，进行单个修饰键与单个普通键组合的操作
    public IKeyboardSimulator ModifiedKeyStroke(User32.VK modifierKeyCode, User32.VK keyCode);

    # 定义一个方法 TextEntry，用于输入字符串
    public IKeyboardSimulator TextEntry(string text);

    # 定义一个方法 TextEntry，用于输入字符
    public IKeyboardSimulator TextEntry(char character);

    # 定义一个方法 Sleep，等待指定的毫秒数
    public IKeyboardSimulator Sleep(int millsecondsTimeout);

    # 定义一个方法 Sleep，等待指定的时间段
    public IKeyboardSimulator Sleep(TimeSpan timeout);
}
```