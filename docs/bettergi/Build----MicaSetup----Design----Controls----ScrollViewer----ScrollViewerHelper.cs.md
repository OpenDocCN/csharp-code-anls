# `.\better-genshin-impact\Build\MicaSetup\Design\Controls\ScrollViewer\ScrollViewerHelper.cs`

```cs
# 定义命名空间和使用的类
﻿using System.Windows;
using System.Windows.Controls;
using System.Windows.Media;

namespace MicaSetup.Design.Controls;

# 定义 ScrollViewerHelper 类
public class ScrollViewerHelper
{
    # 获取附加属性 TrackBrush 的值
    public static Brush GetTrackBrush(DependencyObject obj)
    {
        return (Brush)obj.GetValue(TrackBrushProperty);
    }

    # 设置附加属性 TrackBrush 的值
    public static void SetTrackBrush(DependencyObject obj, Brush value)
    {
        obj.SetValue(TrackBrushProperty, value);
    }

    # 定义附加属性 TrackBrush
    public static readonly DependencyProperty TrackBrushProperty = DependencyProperty.RegisterAttached("TrackBrush", typeof(Brush), typeof(ScrollViewerHelper));

    # 获取附加属性 ThumbBrush 的值
    public static Brush GetThumbBrush(DependencyObject obj)
    {
        return (Brush)obj.GetValue(ThumbBrushProperty);
    }

    # 设置附加属性 ThumbBrush 的值
    public static void SetThumbBrush(DependencyObject obj, Brush value)
    {
        obj.SetValue(ThumbBrushProperty, value);
    }

    # 定义附加属性 ThumbBrush
    public static readonly DependencyProperty ThumbBrushProperty = DependencyProperty.RegisterAttached("ThumbBrush", typeof(Brush), typeof(ScrollViewerHelper));

    # 获取附加属性 ScrollBarCornerRadius 的值
    public static CornerRadius GetScrollBarCornerRadius(DependencyObject obj)
    {
        return (CornerRadius)obj.GetValue(ScrollBarCornerRadiusProperty);
    }

    # 设置附加属性 ScrollBarCornerRadius 的值
    public static void SetScrollBarCornerRadius(DependencyObject obj, CornerRadius value)
    {
        obj.SetValue(ScrollBarCornerRadiusProperty, value);
    }

    # 定义附加属性 ScrollBarCornerRadius
    public static readonly DependencyProperty ScrollBarCornerRadiusProperty = DependencyProperty.RegisterAttached("ScrollBarCornerRadius", typeof(CornerRadius), typeof(ScrollViewerHelper));

    # 获取附加属性 ScrollBarThickness 的值
    public static double GetScrollBarThickness(DependencyObject obj)
    {
        return (double)obj.GetValue(ScrollBarThicknessProperty);
    }

    # 设置附加属性 ScrollBarThickness 的值
    public static void SetScrollBarThickness(DependencyObject obj, double value)
    {
        obj.SetValue(ScrollBarThicknessProperty, value);
    }

    # 定义附加属性 ScrollBarThickness
    public static readonly DependencyProperty ScrollBarThicknessProperty = DependencyProperty.RegisterAttached("ScrollBarThickness", typeof(double), typeof(ScrollViewerHelper));

    # 获取附加属性 ScrollViewerHook 的值
    internal static bool GetScrollViewerHook(DependencyObject obj)
    {
        return (bool)obj.GetValue(ScrollViewerHookProperty);
    }

    # 设置附加属性 ScrollViewerHook 的值
    internal static void SetScrollViewerHook(DependencyObject obj, bool value)
    {
        obj.SetValue(ScrollViewerHookProperty, value);
    }

    # 定义附加属性 ScrollViewerHook，并指定属性更改时的回调方法
    internal static readonly DependencyProperty ScrollViewerHookProperty = DependencyProperty.RegisterAttached("ScrollViewerHook", typeof(bool), typeof(ScrollViewerHelper), new PropertyMetadata(OnScrollViewerHookChanged));

    # 当 ScrollViewerHook 属性更改时调用的方法
    private static void OnScrollViewerHookChanged(DependencyObject d, DependencyPropertyChangedEventArgs e)
    {
        var scrollViewer = d as ScrollViewer;
        scrollViewer!.PreviewMouseWheel += ScrollViewer_PreviewMouseWheel;
    }

    # 处理 ScrollViewer 的 PreviewMouseWheel 事件的方法
    private static void ScrollViewer_PreviewMouseWheel(object sender, System.Windows.Input.MouseWheelEventArgs e)
    # 将 sender 对象转换为 ScrollViewer 类型
    var scrollViewer = sender as ScrollViewer;
    # 初始化处理标志为 true
    var handle = true;

    # 根据滚轮的 Delta 值决定滚动方向
    if (e.Delta > 0)
    {
        # 如果垂直滚动条可见，则不处理滚动事件
        if (scrollViewer!.ComputedVerticalScrollBarVisibility == Visibility.Visible)
        { handle = false; }
        # 如果水平滚动条可见，则向左滚动一行
        else if (scrollViewer.ComputedHorizontalScrollBarVisibility == Visibility.Visible)
            scrollViewer.LineLeft();
        # 如果垂直滚动条没有禁用，则不处理滚动事件
        else if (scrollViewer.VerticalScrollBarVisibility != ScrollBarVisibility.Disabled)
        { handle = false; }
        # 如果水平滚动条没有禁用，则向左滚动一行
        else if (scrollViewer.HorizontalScrollBarVisibility != ScrollBarVisibility.Disabled)
            scrollViewer.LineLeft();
        # 如果以上条件都不满足，则返回
        else
            return;
    }
    else
    {
        # 如果垂直滚动条可见，则不处理滚动事件
        if (scrollViewer!.ComputedVerticalScrollBarVisibility == Visibility.Visible)
        { handle = false; }
        # 如果水平滚动条可见，则向右滚动一行
        else if (scrollViewer.ComputedHorizontalScrollBarVisibility == Visibility.Visible)
            scrollViewer.LineRight();
        # 如果垂直滚动条没有禁用，则不处理滚动事件
        else if (scrollViewer.VerticalScrollBarVisibility != ScrollBarVisibility.Disabled)
        { handle = false; }
        # 如果水平滚动条没有禁用，则向右滚动一行
        else if (scrollViewer.HorizontalScrollBarVisibility != ScrollBarVisibility.Disabled)
            scrollViewer.LineRight();
        # 如果以上条件都不满足，则返回
        else
            return;
    }

    # 如果处理标志仍为 true，则标记事件为已处理
    if (handle)
        e.Handled = true;
这段代码只是一个右花括号 `}`，可能是其他代码块的结束标志。请提供更多代码或上下文，以便准确添加注释。
```