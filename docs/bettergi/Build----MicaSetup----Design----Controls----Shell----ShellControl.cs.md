# `.\better-genshin-impact\Build\MicaSetup\Design\Controls\Shell\ShellControl.cs`

```cs
# 引入必要的命名空间
﻿using System;
using System.Windows;
using System.Windows.Controls;

# 声明命名空间
namespace MicaSetup.Design.Controls;

# 定义一个名为 ShellControl 的类，继承自 ContentControl
public class ShellControl : ContentControl
{
    # 定义一个名为 Route 的属性
    public string Route
    {
        # 获取 Route 属性的值
        get => (string)GetValue(RouteProperty);
        # 设置 Route 属性的值
        set => SetCurrentValue(RouteProperty, value);
    }

    # 声明并初始化 RouteProperty 依赖属性
    public static readonly DependencyProperty RouteProperty = DependencyProperty.Register(nameof(Route), typeof(string), typeof(ShellControl), new(string.Empty));

    # ShellControl 的构造函数
    public ShellControl()
    {
        # 将当前对象的弱引用分配给 Routing.Shell
        Routing.Shell = new WeakReference<ShellControl>(this);
        # 禁用焦点视觉样式
        FocusVisualStyle = null!;
        # 在控件加载后调用 Routing.GoTo 方法
        Loaded += (_, _) => Routing.GoTo(Route);
    }
}
```