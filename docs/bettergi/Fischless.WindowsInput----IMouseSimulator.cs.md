# `.\better-genshin-impact\Fischless.WindowsInput\IMouseSimulator.cs`

```cs
# 定义一个接口 IMouseSimulator，包含与鼠标操作相关的方法
﻿namespace Fischless.WindowsInput;

public interface IMouseSimulator
{
    # 属性：获取与键盘操作相关的模拟器
    public IKeyboardSimulator Keyboard { get; }

    # 方法：通过相对像素值移动鼠标
    public IMouseSimulator MoveMouseBy(int pixelDeltaX, int pixelDeltaY);

    # 方法：移动鼠标到绝对坐标
    public IMouseSimulator MoveMouseTo(double absoluteX, double absoluteY);

    # 方法：移动鼠标到虚拟桌面的绝对坐标
    public IMouseSimulator MoveMouseToPositionOnVirtualDesktop(double absoluteX, double absoluteY);

    # 方法：按下鼠标左键
    public IMouseSimulator LeftButtonDown();

    # 方法：释放鼠标左键
    public IMouseSimulator LeftButtonUp();

    # 方法：点击鼠标左键
    public IMouseSimulator LeftButtonClick();

    # 方法：双击鼠标左键
    public IMouseSimulator LeftButtonDoubleClick();

    # 方法：按下鼠标中键
    public IMouseSimulator MiddleButtonDown();

    # 方法：释放鼠标中键
    public IMouseSimulator MiddleButtonUp();

    # 方法：点击鼠标中键
    public IMouseSimulator MiddleButtonClick();

    # 方法：双击鼠标中键
    public IMouseSimulator MiddleButtonDoubleClick();

    # 方法：按下鼠标右键
    public IMouseSimulator RightButtonDown();

    # 方法：释放鼠标右键
    public IMouseSimulator RightButtonUp();

    # 方法：点击鼠标右键
    public IMouseSimulator RightButtonClick();

    # 方法：双击鼠标右键
    public IMouseSimulator RightButtonDoubleClick();

    # 方法：按下指定的额外鼠标按钮
    public IMouseSimulator XButtonDown(int buttonId);

    # 方法：释放指定的额外鼠标按钮
    public IMouseSimulator XButtonUp(int buttonId);

    # 方法：点击指定的额外鼠标按钮
    public IMouseSimulator XButtonClick(int buttonId);

    # 方法：双击指定的额外鼠标按钮
    public IMouseSimulator XButtonDoubleClick(int buttonId);

    # 方法：垂直滚动鼠标
    public IMouseSimulator VerticalScroll(int scrollAmountInClicks);

    # 方法：水平滚动鼠标
    public IMouseSimulator HorizontalScroll(int scrollAmountInClicks);

    # 方法：使模拟器暂停指定的毫秒数
    public IMouseSimulator Sleep(int millsecondsTimeout);

    # 方法：使模拟器暂停指定的时间间隔
    public IMouseSimulator Sleep(TimeSpan timeout);
}
```