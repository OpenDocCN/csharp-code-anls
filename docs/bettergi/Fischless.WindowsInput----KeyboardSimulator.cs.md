# `.\better-genshin-impact\Fischless.WindowsInput\KeyboardSimulator.cs`

```cs
﻿using Vanara.PInvoke;

namespace Fischless.WindowsInput;

public class KeyboardSimulator : IKeyboardSimulator
{
    // 构造函数，初始化 KeyboardSimulator 对象，检查输入模拟器是否为 null
    public KeyboardSimulator(IInputSimulator inputSimulator)
    {
        _inputSimulator = inputSimulator ?? throw new ArgumentNullException(nameof(inputSimulator));
        _messageDispatcher = new WindowsInputMessageDispatcher(); // 初始化消息分发器
    }

    // 内部构造函数，允许传入自定义的消息分发器，并检查输入模拟器和消息分发器是否为 null
    internal KeyboardSimulator(IInputSimulator inputSimulator, IInputMessageDispatcher messageDispatcher)
    {
        _inputSimulator = inputSimulator ?? throw new ArgumentNullException(nameof(inputSimulator));
        _messageDispatcher = messageDispatcher ?? throw new InvalidOperationException(string.Format("The {0} cannot operate with a null {1}. Please provide a valid {1} instance to use for dispatching {2} messages.", typeof(KeyboardSimulator).Name, typeof(IInputMessageDispatcher).Name, typeof(User32.INPUT).Name));
    }

    // 返回与此键盘模拟器相关联的鼠标模拟器
    public IMouseSimulator Mouse => _inputSimulator.Mouse;

    // 处理修饰键按下的逻辑
    private void ModifiersDown(InputBuilder builder, IEnumerable<User32.VK> modifierKeyCodes)
    {
        if (modifierKeyCodes == null)
        {
            return; // 如果修饰键代码集合为 null，则不执行任何操作
        }
        foreach (User32.VK keyCode in modifierKeyCodes)
        {
            builder.AddKeyDown(keyCode); // 添加修饰键按下事件
        }
    }

    // 处理修饰键释放的逻辑
    private void ModifiersUp(InputBuilder builder, IEnumerable<User32.VK> modifierKeyCodes)
    {
        if (modifierKeyCodes == null)
        {
            return; // 如果修饰键代码集合为 null，则不执行任何操作
        }
        Stack<User32.VK> stack = new(modifierKeyCodes); // 使用栈来按相反顺序释放修饰键
        while (stack.Count > 0)
        {
            builder.AddKeyUp(stack.Pop()); // 从栈中弹出并添加修饰键释放事件
        }
    }

    // 处理键按下的逻辑
    private void KeysPress(InputBuilder builder, IEnumerable<User32.VK> keyCodes, bool? isExtendedKey = null)
    {
        if (keyCodes == null)
        {
            return; // 如果键代码集合为 null，则不执行任何操作
        }
        foreach (User32.VK keyCode in keyCodes)
        {
            builder.AddKeyPress(keyCode, isExtendedKey); // 添加键按下事件
        }
    }

    // 发送模拟的输入事件
    private void SendSimulatedInput(User32.INPUT[] inputList)
    {
        _messageDispatcher.DispatchInput(inputList); // 通过消息分发器发送输入事件
    }

    // 模拟按下一个键
    public IKeyboardSimulator KeyDown(User32.VK keyCode)
    {
        User32.INPUT[] inputList = new InputBuilder().AddKeyDown(keyCode).ToArray(); // 构建按下键的输入事件
        SendSimulatedInput(inputList); // 发送模拟的输入事件
        return this; // 返回当前对象，支持链式调用
    }

    // 模拟按下一个键，并指定是否为扩展键
    public IKeyboardSimulator KeyDown(bool? isExtendedKey, User32.VK keyCode)
    {
        User32.INPUT[] inputList = new InputBuilder().AddKeyDown(keyCode, isExtendedKey).ToArray(); // 构建按下键的输入事件，并指定是否为扩展键
        SendSimulatedInput(inputList); // 发送模拟的输入事件
        return this; // 返回当前对象，支持链式调用
    }

    // 模拟释放一个键
    public IKeyboardSimulator KeyUp(User32.VK keyCode)
    {
        User32.INPUT[] inputList = new InputBuilder().AddKeyUp(keyCode).ToArray(); // 构建释放键的输入事件
        SendSimulatedInput(inputList); // 发送模拟的输入事件
        return this; // 返回当前对象，支持链式调用
    }

    // 模拟释放一个键，并指定是否为扩展键
    public IKeyboardSimulator KeyUp(bool? isExtendedKey, User32.VK keyCode)
    {
        User32.INPUT[] inputList = new InputBuilder().AddKeyUp(keyCode, isExtendedKey).ToArray(); // 构建释放键的输入事件，并指定是否为扩展键
        SendSimulatedInput(inputList); // 发送模拟的输入事件
        return this; // 返回当前对象，支持链式调用
    }
}
    // 模拟按下指定的虚拟键
    public IKeyboardSimulator KeyPress(User32.VK keyCode)
    {
        // 使用 InputBuilder 创建一个包含按键事件的输入列表
        User32.INPUT[] inputList = new InputBuilder().AddKeyPress(keyCode).ToArray();
        // 发送模拟的输入事件
        SendSimulatedInput(inputList);
        // 返回当前实例，以便进行链式调用
        return this;
    }

    // 模拟按下指定的虚拟键，允许指定是否为扩展键
    public IKeyboardSimulator KeyPress(bool? isExtendedKey, User32.VK keyCode)
    {
        // 使用 InputBuilder 创建一个包含按键事件的输入列表，考虑是否为扩展键
        User32.INPUT[] inputList = new InputBuilder().AddKeyPress(keyCode, isExtendedKey).ToArray();
        // 发送模拟的输入事件
        SendSimulatedInput(inputList);
        // 返回当前实例，以便进行链式调用
        return this;
    }

    // 模拟按下多个指定的虚拟键
    public IKeyboardSimulator KeyPress(params User32.VK[] keyCodes)
    {
        // 创建 InputBuilder 实例
        InputBuilder inputBuilder = new();
        // 使用 KeysPress 方法将按键事件添加到输入构建器中
        KeysPress(inputBuilder, keyCodes);
        // 发送模拟的输入事件
        SendSimulatedInput(inputBuilder.ToArray());
        // 返回当前实例，以便进行链式调用
        return this;
    }

    // 模拟按下多个指定的虚拟键，允许指定是否为扩展键
    public IKeyboardSimulator KeyPress(bool? isExtendedKey, params User32.VK[] keyCodes)
    {
        // 创建 InputBuilder 实例
        InputBuilder inputBuilder = new();
        // 使用 KeysPress 方法将按键事件添加到输入构建器中，考虑是否为扩展键
        KeysPress(inputBuilder, keyCodes, isExtendedKey);
        // 发送模拟的输入事件
        SendSimulatedInput(inputBuilder.ToArray());
        // 返回当前实例，以便进行链式调用
        return this;
    }

    // 模拟按下一个修饰键和一个普通键
    public IKeyboardSimulator ModifiedKeyStroke(User32.VK modifierKeyCode, User32.VK keyCode)
    {
        // 调用 ModifiedKeyStroke 方法，传入修饰键和普通键
        ModifiedKeyStroke(
        [
            modifierKeyCode,
        ],
        [
            keyCode,
        ]);
        // 返回当前实例，以便进行链式调用
        return this;
    }

    // 模拟按下多个修饰键和一个普通键
    public IKeyboardSimulator ModifiedKeyStroke(IEnumerable<User32.VK> modifierKeyCodes, User32.VK keyCode)
    {
        // 调用 ModifiedKeyStroke 方法，传入修饰键和普通键
        ModifiedKeyStroke(modifierKeyCodes,
        [
            keyCode
        ]);
        // 返回当前实例，以便进行链式调用
        return this;
    }

    // 模拟按下一个修饰键和多个普通键
    public IKeyboardSimulator ModifiedKeyStroke(User32.VK modifierKey, IEnumerable<User32.VK> keyCodes)
    {
        // 调用 ModifiedKeyStroke 方法，传入修饰键和多个普通键
        ModifiedKeyStroke(
        [
            modifierKey
        ], keyCodes);
        // 返回当前实例，以便进行链式调用
        return this;
    }

    // 模拟按下多个修饰键和多个普通键
    public IKeyboardSimulator ModifiedKeyStroke(IEnumerable<User32.VK> modifierKeyCodes, IEnumerable<User32.VK> keyCodes)
    {
        // 创建 InputBuilder 实例
        InputBuilder inputBuilder = new();
        // 使用 ModifiersDown 方法将修饰键按下事件添加到输入构建器中
        ModifiersDown(inputBuilder, modifierKeyCodes);
        // 使用 KeysPress 方法将普通键按下事件添加到输入构建器中
        KeysPress(inputBuilder, keyCodes);
        // 使用 ModifiersUp 方法将修饰键抬起事件添加到输入构建器中
        ModifiersUp(inputBuilder, modifierKeyCodes);
        // 发送模拟的输入事件
        SendSimulatedInput(inputBuilder.ToArray());
        // 返回当前实例，以便进行链式调用
        return this;
    }

    // 模拟输入指定的文本字符串
    public IKeyboardSimulator TextEntry(string text)
    {
        // 检查文本长度是否超出限制
        if (text.Length > 2147483647L)
        {
            // 抛出异常，文本过长
            throw new ArgumentException(string.Format("The text parameter is too long. It must be less than {0} characters.", 2147483647U), "text");
        }
        // 使用 InputBuilder 创建包含文本字符的输入列表
        User32.INPUT[] inputList = new InputBuilder().AddCharacters(text).ToArray();
        // 发送模拟的输入事件
        SendSimulatedInput(inputList);
        // 返回当前实例，以便进行链式调用
        return this;
    }

    // 模拟输入一个字符
    public IKeyboardSimulator TextEntry(char character)
    {
        // 使用 InputBuilder 创建包含单个字符的输入列表
        User32.INPUT[] inputList = new InputBuilder().AddCharacter(character).ToArray();
        // 发送模拟的输入事件
        SendSimulatedInput(inputList);
        // 返回当前实例，以便进行链式调用
        return this;
    }

    // 暂停指定的毫秒数
    public IKeyboardSimulator Sleep(int millsecondsTimeout)
    {
        // 使当前线程睡眠指定的毫秒数
        Thread.Sleep(millsecondsTimeout);
        // 返回当前实例，以便进行链式调用
        return this;
    }

    // 暂停指定的时间间隔
    public IKeyboardSimulator Sleep(TimeSpan timeout)
    {
        // 使当前线程睡眠指定的时间间隔
        Thread.Sleep(timeout);
        // 返回当前实例，以便进行链式调用
        return this;
    }
    // 声明一个只读的输入模拟器接口，用于模拟用户输入
    private readonly IInputSimulator _inputSimulator;

    // 声明一个只读的消息派发器接口，用于处理输入消息的分发
    private readonly IInputMessageDispatcher _messageDispatcher;
# 结束一个代码块，可能是函数、条件语句、循环等
}
```