# `.\better-genshin-impact\Fischless.WindowsInput\MouseSimulator.cs`

```cs
﻿using Vanara.PInvoke;

namespace Fischless.WindowsInput;

public class MouseSimulator : IMouseSimulator
{
    // 构造函数，接受一个输入模拟器实例，并初始化鼠标模拟器
    public MouseSimulator(IInputSimulator inputSimulator)
    {
        _inputSimulator = inputSimulator ?? throw new ArgumentNullException(nameof(inputSimulator));
        _messageDispatcher = new WindowsInputMessageDispatcher();
    }

    // 内部构造函数，接受一个输入模拟器和一个消息分发器实例，进行初始化
    internal MouseSimulator(IInputSimulator inputSimulator, IInputMessageDispatcher messageDispatcher)
    {
        _inputSimulator = inputSimulator ?? throw new ArgumentNullException(nameof(inputSimulator));
        _messageDispatcher = messageDispatcher ?? throw new InvalidOperationException(string.Format("The {0} cannot operate with a null {1}. Please provide a valid {1} instance to use for dispatching {2} messages.", nameof(MouseSimulator), typeof(IInputMessageDispatcher).Name, typeof(User32.INPUT).Name));
    }

    // 属性，返回输入模拟器的键盘模拟器
    public IKeyboardSimulator Keyboard => _inputSimulator.Keyboard;

    // 私有方法，发送模拟输入到消息分发器
    private void SendSimulatedInput(User32.INPUT[] inputList)
    {
        _messageDispatcher.DispatchInput(inputList);
    }

    // 移动鼠标相对位置
    public IMouseSimulator MoveMouseBy(int pixelDeltaX, int pixelDeltaY)
    {
        User32.INPUT[] inputList = new InputBuilder().AddRelativeMouseMovement(pixelDeltaX, pixelDeltaY).ToArray();
        SendSimulatedInput(inputList);
        return this;
    }

    // 移动鼠标到绝对位置
    public IMouseSimulator MoveMouseTo(double absoluteX, double absoluteY)
    {
        User32.INPUT[] inputList = new InputBuilder().AddAbsoluteMouseMovement((int)Math.Truncate(absoluteX), (int)Math.Truncate(absoluteY)).ToArray();
        SendSimulatedInput(inputList);
        return this;
    }

    // 移动鼠标到虚拟桌面上的绝对位置
    public IMouseSimulator MoveMouseToPositionOnVirtualDesktop(double absoluteX, double absoluteY)
    {
        User32.INPUT[] inputList = new InputBuilder().AddAbsoluteMouseMovementOnVirtualDesktop((int)Math.Truncate(absoluteX), (int)Math.Truncate(absoluteY)).ToArray();
        SendSimulatedInput(inputList);
        return this;
    }

    // 按下左键
    public IMouseSimulator LeftButtonDown()
    {
        User32.INPUT[] inputList = new InputBuilder().AddMouseButtonDown(MouseButton.LeftButton).ToArray();
        SendSimulatedInput(inputList);
        return this;
    }

    // 释放左键
    public IMouseSimulator LeftButtonUp()
    {
        User32.INPUT[] inputList = new InputBuilder().AddMouseButtonUp(MouseButton.LeftButton).ToArray();
        this.SendSimulatedInput(inputList);
        return this;
    }

    // 左键点击
    public IMouseSimulator LeftButtonClick()
    {
        User32.INPUT[] inputList = new InputBuilder().AddMouseButtonClick(MouseButton.LeftButton).ToArray();
        SendSimulatedInput(inputList);
        return this;
    }

    // 左键双击
    public IMouseSimulator LeftButtonDoubleClick()
    {
        User32.INPUT[] inputList = new InputBuilder().AddMouseButtonDoubleClick(MouseButton.LeftButton).ToArray();
        SendSimulatedInput(inputList);
        return this;
    }

    // 中键按下
    # 创建一个包含中键按下的输入列表，并发送模拟输入
    User32.INPUT[] inputList = new InputBuilder().AddMouseButtonDown(MouseButton.MiddleButton).ToArray();
    SendSimulatedInput(inputList);
    return this;

    # 创建一个包含中键释放的输入列表，并发送模拟输入
    public IMouseSimulator MiddleButtonUp()
    {
        User32.INPUT[] inputList = new InputBuilder().AddMouseButtonUp(MouseButton.MiddleButton).ToArray();
        this.SendSimulatedInput(inputList);
        return this;
    }

    # 创建一个包含中键点击的输入列表，并发送模拟输入
    public IMouseSimulator MiddleButtonClick()
    {
        User32.INPUT[] inputList = new InputBuilder().AddMouseButtonClick(MouseButton.MiddleButton).ToArray();
        SendSimulatedInput(inputList);
        return this;
    }

    # 创建一个包含中键双击的输入列表，并发送模拟输入
    public IMouseSimulator MiddleButtonDoubleClick()
    {
        User32.INPUT[] inputList = new InputBuilder().AddMouseButtonDoubleClick(MouseButton.MiddleButton).ToArray();
        SendSimulatedInput(inputList);
        return this;
    }

    # 创建一个包含右键按下的输入列表，并发送模拟输入
    public IMouseSimulator RightButtonDown()
    {
        User32.INPUT[] inputList = new InputBuilder().AddMouseButtonDown(MouseButton.RightButton).ToArray();
        SendSimulatedInput(inputList);
        return this;
    }

    # 创建一个包含右键释放的输入列表，并发送模拟输入
    public IMouseSimulator RightButtonUp()
    {
        User32.INPUT[] inputList = new InputBuilder().AddMouseButtonUp(MouseButton.RightButton).ToArray();
        SendSimulatedInput(inputList);
        return this;
    }

    # 创建一个包含右键点击的输入列表，并发送模拟输入
    public IMouseSimulator RightButtonClick()
    {
        User32.INPUT[] inputList = new InputBuilder().AddMouseButtonClick(MouseButton.RightButton).ToArray();
        SendSimulatedInput(inputList);
        return this;
    }

    # 创建一个包含右键双击的输入列表，并发送模拟输入
    public IMouseSimulator RightButtonDoubleClick()
    {
        User32.INPUT[] inputList = new InputBuilder().AddMouseButtonDoubleClick(MouseButton.RightButton).ToArray();
        SendSimulatedInput(inputList);
        return this;
    }

    # 创建一个包含 X 按钮按下的输入列表，并发送模拟输入
    public IMouseSimulator XButtonDown(int buttonId)
    {
        User32.INPUT[] inputList = new InputBuilder().AddMouseXButtonDown(buttonId).ToArray();
        SendSimulatedInput(inputList);
        return this;
    }

    # 创建一个包含 X 按钮释放的输入列表，并发送模拟输入
    public IMouseSimulator XButtonUp(int buttonId)
    {
        User32.INPUT[] inputList = new InputBuilder().AddMouseXButtonUp(buttonId).ToArray();
        SendSimulatedInput(inputList);
        return this;
    }

    # 创建一个包含 X 按钮点击的输入列表，并发送模拟输入
    public IMouseSimulator XButtonClick(int buttonId)
    {
        User32.INPUT[] inputList = new InputBuilder().AddMouseXButtonClick(buttonId).ToArray();
        SendSimulatedInput(inputList);
        return this;
    }

    # 创建一个包含 X 按钮双击的输入列表，并发送模拟输入
    public IMouseSimulator XButtonDoubleClick(int buttonId)
    {
        User32.INPUT[] inputList = new InputBuilder().AddMouseXButtonDoubleClick(buttonId).ToArray();
        SendSimulatedInput(inputList);
        return this;
    }

    # 创建一个包含垂直滚轮滚动的输入列表，并发送模拟输入
    public IMouseSimulator VerticalScroll(int scrollAmountInClicks)
    {
        User32.INPUT[] inputList = new InputBuilder().AddMouseVerticalWheelScroll(scrollAmountInClicks * 120).ToArray();
        SendSimulatedInput(inputList);
        return this;
    }
    // 滚动鼠标水平滚轮指定的点击次数
    public IMouseSimulator HorizontalScroll(int scrollAmountInClicks)
    {
        // 创建一个包含模拟鼠标水平滚轮滚动事件的输入列表，滚动量是点击次数乘以 120（每次点击对应的滚动量）
        User32.INPUT[] inputList = new InputBuilder().AddMouseHorizontalWheelScroll(scrollAmountInClicks * 120).ToArray();
        // 发送模拟输入事件
        SendSimulatedInput(inputList);
        // 返回当前实例以支持链式调用
        return this;
    }

    // 使当前线程休眠指定的毫秒数
    public IMouseSimulator Sleep(int millsecondsTimeout)
    {
        // 当前线程休眠指定的毫秒数
        Thread.Sleep(millsecondsTimeout);
        // 返回当前实例以支持链式调用
        return this;
    }

    // 使当前线程休眠指定的时间间隔
    public IMouseSimulator Sleep(TimeSpan timeout)
    {
        // 当前线程休眠指定的时间间隔
        Thread.Sleep(timeout);
        // 返回当前实例以支持链式调用
        return this;
    }

    // 定义只读字段，表示输入模拟器
    private readonly IInputSimulator _inputSimulator;

    // 定义只读字段，表示输入消息调度器
    private readonly IInputMessageDispatcher _messageDispatcher;
# 结束当前代码块的标记，通常在函数、类或控制结构中使用
}
```