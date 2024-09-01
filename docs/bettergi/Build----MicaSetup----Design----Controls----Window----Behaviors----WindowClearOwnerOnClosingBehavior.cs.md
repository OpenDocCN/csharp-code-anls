# `.\better-genshin-impact\Build\MicaSetup\Design\Controls\Window\Behaviors\WindowClearOwnerOnClosingBehavior.cs`

```cs
# 引入 Microsoft.Xaml.Behaviors 命名空间，用于实现行为模式
using Microsoft.Xaml.Behaviors;
# 引入系统命名空间，用于基础功能
using System;
# 引入组件模型命名空间，用于组件的事件处理
using System.ComponentModel;
# 引入调试命名空间，用于调试功能
using System.Diagnostics;
# 引入窗口功能命名空间
using System.Windows;

# 定义一个行为类，专门用于处理窗口关闭时清除窗口拥有者
namespace MicaSetup.Design.Controls;

# 继承自 Behavior<Window>，允许将该行为附加到窗口对象上
public sealed class WindowClearOwnerOnClosingBehavior : Behavior<Window>
{
    # 重写 OnAttached 方法，当行为附加到窗口时调用
    protected override void OnAttached()
    {
        # 订阅窗口的 Closing 事件，指定事件处理方法为 OnWindowClosing
        AssociatedObject.Closing += OnWindowClosing;
        # 调用基类的 OnAttached 方法
        base.OnAttached();
    }

    # 重写 OnDetaching 方法，当行为从窗口上分离时调用
    protected override void OnDetaching()
    {
        # 取消订阅窗口的 Closing 事件，解除事件处理方法
        AssociatedObject.Closing -= OnWindowClosing;
        # 调用基类的 OnDetaching 方法
        base.OnDetaching();
    }

    # 处理窗口关闭事件的方法
    private void OnWindowClosing(object? sender, CancelEventArgs e)
    {
        # 如果事件没有被取消
        if (!e.Cancel)
        {
            try
            {
                # 如果窗口有拥有者
                if (AssociatedObject.Owner != null)
                {
                    # 将窗口的拥有者设为 null
                    AssociatedObject.Owner = null!;
                }
            }
            catch (Exception ex)
            {
                # 捕捉异常并输出调试信息，说明不应该将此行为附加到窗口的 ShowDialog 调用中
                Debug.WriteLine($"Don't attach this {nameof(WindowClearOwnerOnClosingBehavior)} for {nameof(Window)} called from {nameof(Window.ShowDialog)}.", ex);
                # 如果调试器附加，则中断执行，方便调试
                if (Debugger.IsAttached)
                {
                    Debugger.Break();
                }
            }
        }
    }
}
```