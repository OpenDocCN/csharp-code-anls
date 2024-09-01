# `.\better-genshin-impact\Build\MicaSetup\Design\Controls\Window\Behaviors\WindowTitleIconBehavior.cs`

```cs
# 引入必要的命名空间和库
﻿using MicaSetup.Helper;
using MicaSetup.Natives;
using Microsoft.Xaml.Behaviors;
using System;
using System.Windows;
using System.Windows.Input;

namespace MicaSetup.Design.Controls;

# 定义一个名为 WindowTitleIconBehavior 的行为类，继承自 Behavior<FrameworkElement>
public sealed class WindowTitleIconBehavior : Behavior<FrameworkElement>
{
    # 当行为附加到目标对象时调用
    protected override void OnAttached()
    {
        # 订阅目标对象的 Loaded 事件，处理事件时调用 Loaded 方法
        AssociatedObject.Loaded += Loaded;
        # 调用基类的 OnAttached 方法
        base.OnAttached();
    }

    # 当行为从目标对象分离时调用
    protected override void OnDetaching()
    {
        # 取消订阅目标对象的 Loaded 事件
        AssociatedObject.Loaded -= Loaded;
        # 调用基类的 OnDetaching 方法
        base.OnDetaching();
    }

    # 处理 Loaded 事件的方法
    private void Loaded(object sender, EventArgs e)
    {
        # 在目标对象上注册为标题图标
        AssociatedObject.RegisterAsTitleIcon();
    }
}

# 扩展 UIElement 类，增加 RegisterAsTitleIcon 方法
public static class RegisterAsTitleIconBehaviorExtension
{
    # 扩展方法，将鼠标左键点击事件注册到 UIElement 上
    public static void RegisterAsTitleIcon(this UIElement self)
    {
        # 鼠标左键点击事件处理程序
        self.MouseLeftButtonDown += (s, e) =>
        {
            # 如果事件源是 UIElement 类型，并且获取到其所在的窗口
            if (s is UIElement titleHeader && Window.GetWindow(titleHeader) is Window window)
            {
                # 如果左键被按下
                if (e.LeftButton == MouseButtonState.Pressed)
                {
                    # 获取鼠标位置，并显示系统菜单
                    if (User32.GetCursorPos(out POINT pt))
                    {
                        SystemCommands.ShowSystemMenu(window, new Point(DpiHelper.CalcDPiX(pt.X), DpiHelper.CalcDPiY(pt.Y)));
                    }
                }
            }
        };
    }
}
```