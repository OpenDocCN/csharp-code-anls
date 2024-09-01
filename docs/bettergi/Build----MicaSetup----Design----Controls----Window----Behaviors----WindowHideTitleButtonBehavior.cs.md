# `.\better-genshin-impact\Build\MicaSetup\Design\Controls\Window\Behaviors\WindowHideTitleButtonBehavior.cs`

```cs
# 使用 MicaSetup.Natives 命名空间中的类型和方法
using MicaSetup.Natives;
# 使用 Microsoft.Xaml.Behaviors 命名空间中的行为基础类
using Microsoft.Xaml.Behaviors;
# 引入系统和 WPF 相关命名空间
using System;
using System.Windows;
using System.Windows.Interop;

# 定义 MicaSetup.Design.Controls 命名空间
namespace MicaSetup.Design.Controls;

# 创建一个行为类，用于控制窗口隐藏标题按钮
public sealed class WindowHideTitleButtonBehavior : Behavior<Window>
{
    # 当行为附加到目标对象时调用
    protected override void OnAttached()
    {
        # 订阅窗口的 SourceInitialized 事件，事件处理程序为 OnSourceInitialized
        AssociatedObject.SourceInitialized += OnSourceInitialized;
        # 调用基类的 OnAttached 方法
        base.OnAttached();
    }

    # 当行为从目标对象中分离时调用
    protected override void OnDetaching()
    {
        # 取消订阅窗口的 SourceInitialized 事件
        AssociatedObject.Loaded -= OnSourceInitialized;
        # 调用基类的 OnDetaching 方法
        base.OnDetaching();
    }

    # 事件处理程序，用于在窗口初始化时隐藏窗口按钮
    private void OnSourceInitialized(object? sender, EventArgs e)
    {
        # 使用 NativeMethods.HideAllWindowButton 方法隐藏窗口的所有按钮
        NativeMethods.HideAllWindowButton(new WindowInteropHelper(AssociatedObject).Handle);
    }
}
```