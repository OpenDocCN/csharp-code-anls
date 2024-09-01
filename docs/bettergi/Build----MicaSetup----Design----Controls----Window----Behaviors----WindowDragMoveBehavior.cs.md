# `.\better-genshin-impact\Build\MicaSetup\Design\Controls\Window\Behaviors\WindowDragMoveBehavior.cs`

```cs
# 引入 Microsoft.Xaml.Behaviors 命名空间，提供附加行为的功能
﻿using Microsoft.Xaml.Behaviors;
# 引入系统命名空间，提供基础功能
using System;
# 引入 WPF 窗口相关功能
using System.Windows;

# 声明一个命名空间，用于组织代码
namespace MicaSetup.Design.Controls;

# 定义一个行为类，用于处理窗口拖动功能
public sealed class WindowDragMoveBehavior : Behavior<FrameworkElement>
{
    # 当行为附加到关联的对象时调用
    protected override void OnAttached()
    {
        # 订阅 MouseLeftButtonDown 事件，以便响应鼠标左键按下
        AssociatedObject.MouseLeftButtonDown += MouseLeftButtonDown;
        # 调用基类的 OnAttached 方法
        base.OnAttached();
    }

    # 当行为从关联的对象上分离时调用
    protected override void OnDetaching()
    {
        # 取消订阅 MouseLeftButtonDown 事件
        AssociatedObject.MouseLeftButtonDown -= MouseLeftButtonDown;
        # 调用基类的 OnDetaching 方法
        base.OnDetaching();
    }

    # 处理 MouseLeftButtonDown 事件的方法
    private void MouseLeftButtonDown(object sender, EventArgs e)
    {
        # 确保发送者是 UIElement 类型
        if (sender is UIElement ui)
        {
            # 获取 UIElement 所在的窗口，并调用 DragMove 方法使窗口响应拖动操作
            Window.GetWindow(ui)?.DragMove();
        }
    }
}
```