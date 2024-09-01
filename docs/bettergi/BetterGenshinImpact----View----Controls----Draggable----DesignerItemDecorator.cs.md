# `.\better-genshin-impact\BetterGenshinImpact\View\Controls\Draggable\DesignerItemDecorator.cs`

```cs
# 导入自定义的 Adorner 类和其他需要的 WPF 类
﻿using BetterGenshinImpact.View.Controls.Adorners;
using System.Windows;
using System.Windows.Controls;
using System.Windows.Documents;

namespace BetterGenshinImpact.View.Controls;

# 定义一个自定义控件 DesignerItemDecorator，继承自 Control
public class DesignerItemDecorator : Control
{
    # 定义一个 Adorner 对象的私有字段
    private Adorner? adorner;

    # 定义 ShowDecorator 属性用于控制装饰器的显示
    public bool ShowDecorator
    {
        get { return (bool)GetValue(ShowDecoratorProperty); }
        set { SetValue(ShowDecoratorProperty, value); }
    }

    # 注册 ShowDecorator 依赖属性
    public static readonly DependencyProperty ShowDecoratorProperty =
        DependencyProperty.Register(nameof(ShowDecorator), typeof(bool), typeof(DesignerItemDecorator),
        new FrameworkPropertyMetadata(false, new PropertyChangedCallback(OnShowDecoratorChanged)));

    # 构造函数，订阅 Unloaded 事件
    public DesignerItemDecorator()
    {
        Unloaded += OnDesignerItemDecoratorUnloaded;
    }

    # 隐藏 Adorner
    private void HideAdorner()
    {
        if (adorner is not null)
        {
            adorner.Visibility = Visibility.Hidden;
        }
    }

    # 显示 Adorner
    private void ShowAdorner()
    {
        if (adorner is null)
        {
            # 获取与当前控件相关联的 AdornerLayer
            AdornerLayer adornerLayer = AdornerLayer.GetAdornerLayer(this);

            if (adornerLayer is not null)
            {
                # 从 DataContext 中获取 ContentControl 对象
                ContentControl? designerItem = DataContext as ContentControl;
                # Canvas? canvas = VisualTreeHelper.GetParent(designerItem) as Canvas;
                # 创建并添加一个新的 ResizeRotateAdorner 实例到 AdornerLayer
                adorner = new ResizeRotateAdorner(designerItem);
                adornerLayer.Add(adorner);

                # 根据 ShowDecorator 属性决定 Adorner 的可见性
                adorner.Visibility = ShowDecorator ? Visibility.Visible : Visibility.Hidden;
            }
        }
        else
        {
            # 如果 Adorner 已存在，设置其为可见
            adorner.Visibility = Visibility.Visible;
        }
    }

    # 处理控件卸载事件，从 AdornerLayer 中移除 Adorner
    private void OnDesignerItemDecoratorUnloaded(object sender, RoutedEventArgs e)
    {
        if (adorner is not null)
        {
            AdornerLayer.GetAdornerLayer(this)?.Remove(adorner);
            adorner = null;
        }
    }

    # 响应 ShowDecorator 属性变化
    private static void OnShowDecoratorChanged(DependencyObject d, DependencyPropertyChangedEventArgs e)
    {
        DesignerItemDecorator decorator = (DesignerItemDecorator)d;
        bool showDecorator = (bool)e.NewValue;

        if (showDecorator)
        {
            decorator.ShowAdorner();
        }
        else
        {
            decorator.HideAdorner();
        }
    }
}
```