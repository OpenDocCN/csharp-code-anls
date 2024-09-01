# `.\better-genshin-impact\Build\MicaSetup\Design\Controls\Window\Behaviors\WindowRestorePathBehavior.cs`

```cs
# 引入必要的命名空间
﻿using Microsoft.Xaml.Behaviors;
using System;
using System.Windows;
using System.Windows.Media;
using System.Windows.Shapes;

namespace MicaSetup.Design.Controls;

# 定义一个继承自 Behavior<FrameworkElement> 的类，用于窗口状态恢复行为
public sealed class WindowRestorePathBehavior : Behavior<FrameworkElement>
{
    # 保存前一个窗口状态的字段
    private WindowState windowStatePrev = WindowState.Normal;

    # 当关联的对象附加时调用
    protected override void OnAttached()
    {
        # 订阅 AssociatedObject 的 Loaded 事件
        AssociatedObject.Loaded += Loaded;
        # 调用基类方法
        base.OnAttached();
    }

    # 当关联的对象分离时调用
    protected override void OnDetaching()
    {
        # 取消订阅 AssociatedObject 的 Loaded 事件
        AssociatedObject.Loaded -= Loaded;
        # 调用基类方法
        base.OnDetaching();
    }

    # 处理 Loaded 事件的方法
    private void Loaded(object sender, EventArgs e)
    {
        # 检查发送者是否是 UIElement，并获取其所在的窗口
        if (sender is UIElement maximizeButtonContent && Window.GetWindow(maximizeButtonContent) is Window window)
        {
            # 当窗口大小发生变化时
            window.SizeChanged += (s, e) =>
            {
                # 如果窗口状态发生变化
                if (windowStatePrev != window.WindowState)
                {
                    # 检查 maximizeButtonContent 是否是 Path 类型
                    if (maximizeButtonContent is Path path)
                    {
                        # 如果窗口最大化，设置路径数据为最大化图标
                        if (window.WindowState == WindowState.Maximized)
                        {
                            path.Data = Geometry.Parse("M0 0.2 L0.8 0.2 M0 01 L0.8 1 M0.8 1 L0.8 0.2 M0 0.2 L0 1 M0.3 0 L0.95 0 M01 0.05 L1 0.7");
                        }
                        # 如果窗口未最大化，设置路径数据为还原图标
                        else
                        {
                            path.Data = Geometry.Parse("M0.025 0 L0.975 0 M0.025 1 L0.975 1 M1 0.975 L1 0.025 M0 0.025 L0 0.975");
                        }
                    }
                    # 更新保存的窗口状态
                    windowStatePrev = window.WindowState;
                }
            };
        }
    }
}
```