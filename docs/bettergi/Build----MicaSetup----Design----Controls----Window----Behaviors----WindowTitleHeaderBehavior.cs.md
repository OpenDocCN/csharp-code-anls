# `.\better-genshin-impact\Build\MicaSetup\Design\Controls\Window\Behaviors\WindowTitleHeaderBehavior.cs`

```cs
﻿using MicaSetup.Helper;  // 引入 MicaSetup.Helper 命名空间
using MicaSetup.Natives;  // 引入 MicaSetup.Natives 命名空间
using Microsoft.Xaml.Behaviors;  // 引入 Microsoft.Xaml.Behaviors 命名空间
using System;  // 引入 System 命名空间
using System.Windows;  // 引入 System.Windows 命名空间
using System.Windows.Input;  // 引入 System.Windows.Input 命名空间

namespace MicaSetup.Design.Controls;  // 定义 MicaSetup.Design.Controls 命名空间

// 定义一个封装类 WindowTitleHeaderBehavior，继承自 Behavior<FrameworkElement>
public sealed class WindowTitleHeaderBehavior : Behavior<FrameworkElement>
{
    // 当行为附加到关联对象时调用
    protected override void OnAttached()
    {
        // 为关联对象的 Loaded 事件添加事件处理程序 Loaded
        AssociatedObject.Loaded += Loaded;
        // 调用基类的 OnAttached 方法
        base.OnAttached();
    }

    // 当行为从关联对象分离时调用
    protected override void OnDetaching()
    {
        // 从关联对象的 Loaded 事件中移除事件处理程序 Loaded
        AssociatedObject.Loaded -= Loaded;
        // 调用基类的 OnDetaching 方法
        base.OnDetaching();
    }

    // 处理 Loaded 事件的方法
    private void Loaded(object sender, EventArgs e)
    {
        // 将关联对象注册为标题头
        AssociatedObject.RegisterAsTitleHeader();
    }
}

// 定义一个静态扩展类 RegisterAsTitleHeaderBehaviorExtension
file static class RegisterAsTitleHeaderBehaviorExtension
{
    // 为 UIElement 类型添加扩展方法 RegisterAsTitleHeader
    public static void RegisterAsTitleHeader(this UIElement self)
    {
        // 当鼠标左键点击时触发的事件处理程序
        self.MouseLeftButtonDown += (s, e) =>
        {
            // 检查事件源是否为 UIElement 并获取其窗口
            if (s is UIElement titleHeader && Window.GetWindow(titleHeader) is Window window)
            {
                // 双击时进行窗口的最大化或还原
                if (e.ClickCount == 2)
                {
                    if (window.ResizeMode == ResizeMode.CanResize || window.ResizeMode == ResizeMode.CanResizeWithGrip)
                    {
                        switch (window.WindowState)
                        {
                            case WindowState.Normal:
                                // 将窗口最大化
                                WindowSystemCommands.MaximizeWindow(window);
                                break;

                            case WindowState.Maximized:
                                // 将窗口还原
                                WindowSystemCommands.RestoreWindow(window);
                                break;
                        }
                    }
                }
                else
                {
                    // 单击并按下左键时，快速还原窗口
                    if (e.LeftButton == MouseButtonState.Pressed)
                    {
                        WindowSystemCommands.FastRestoreWindow(window);
                    }
                }
            }
        };

        // 当鼠标右键点击时触发的事件处理程序
        self.MouseRightButtonDown += (s, e) =>
        {
            // 检查事件源是否为 UIElement 并获取其窗口
            if (s is UIElement titleHeader && Window.GetWindow(titleHeader) is Window window)
            {
                // 检查窗口的光标状态
                if (window.Cursor != null && window.Cursor.ToString() == Cursors.None.ToString())
                {
                    return;  // 如果光标状态为空，则返回
                }
                // 按下右键时显示系统菜单
                if (e.RightButton == MouseButtonState.Pressed)
                {
                    // 获取光标位置并显示系统菜单
                    if (User32.GetCursorPos(out POINT pt))
                    {
                        e.Handled = true;  // 标记事件为已处理
                        SystemCommands.ShowSystemMenu(window, new Point(DpiHelper.CalcDPiX(pt.X), DpiHelper.CalcDPiY(pt.Y)));
                    }
                }
            }
        };
    }
}
```