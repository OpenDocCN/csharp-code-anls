# `.\better-genshin-impact\Build\MicaSetup\Design\Controls\ScrollViewer\SmoothScrollViewer.cs`

```cs
# 引入所需的命名空间
﻿using System;
using System.Windows;
using System.Windows.Controls;
using System.Windows.Input;
using System.Windows.Media;
using System.Windows.Media.Animation;

# 定义命名空间
namespace MicaSetup.Design.Controls;

# 定义 SmoothScrollViewer 类，继承自 ScrollViewer
public class SmoothScrollViewer : ScrollViewer
{
    # 定义私有字段，用于存储总的垂直和水平偏移量以及动画是否正在运行
    private double _totalVerticalOffset;
    private double _totalHorizontalOffset;
    private bool _isRunning;

    # 定义 Orientation 属性，获取或设置滚动方向
    public Orientation Orientation
    {
        get => (Orientation)GetValue(OrientationProperty); # 获取 Orientation 属性的值
        set => SetValue(OrientationProperty, value); # 设置 Orientation 属性的值
    }

    # 定义 Orientation 属性的依赖属性
    public static readonly DependencyProperty OrientationProperty = DependencyProperty.Register(nameof(Orientation), typeof(Orientation), typeof(SmoothScrollViewer), new PropertyMetadata(Orientation.Vertical));

    # 定义 CanMouseWheel 属性，获取或设置是否允许鼠标滚轮操作
    public bool CanMouseWheel
    {
        get => (bool)GetValue(CanMouseWheelProperty); # 获取 CanMouseWheel 属性的值
        set => SetValue(CanMouseWheelProperty, value); # 设置 CanMouseWheel 属性的值
    }

    # 定义 CanMouseWheel 属性的依赖属性
    public static readonly DependencyProperty CanMouseWheelProperty = DependencyProperty.Register(nameof(CanMouseWheel), typeof(bool), typeof(SmoothScrollViewer), new PropertyMetadata(true));

    # 重写 OnMouseWheel 方法以处理鼠标滚轮事件
    protected override void OnMouseWheel(MouseWheelEventArgs e)
    {
        # 如果垂直视口高度加上当前垂直偏移量大于等于内容的总高度且滚轮向下滚动，则不做处理
        if (ViewportHeight + VerticalOffset >= ExtentHeight && e.Delta <= 0)
        {
            return;
        }

        # 如果当前垂直偏移量为 0 且滚轮向上滚动，则不做处理
        if (VerticalOffset == 0 && e.Delta >= 0)
        {
            return;
        }

        # 如果不允许鼠标滚轮操作，则不做处理
        if (!CanMouseWheel)
        {
            return;
        }

        # 如果惯性滚动未启用，则进行平滑滚动处理
        if (!IsInertiaEnabled)
        {
            # 如果滚动方向为垂直，则调用基类的 OnMouseWheel 方法
            if (Orientation == Orientation.Vertical)
            {
                base.OnMouseWheel(e);
            }
            else
            {
                # 记录当前水平偏移量，并更新水平偏移量
                _totalHorizontalOffset = HorizontalOffset;
                CurrentHorizontalOffset = HorizontalOffset;
                # 计算新的水平偏移量，确保其在有效范围内
                _totalHorizontalOffset = Math.Min(Math.Max(0, _totalHorizontalOffset - e.Delta), ScrollableWidth);
                CurrentHorizontalOffset = _totalHorizontalOffset;
            }

            return;
        }

        # 设置事件已处理标志
        e.Handled = true;

        # 如果滚动方向为垂直，则进行垂直平滑滚动处理
        if (Orientation == Orientation.Vertical)
        {
            if (!_isRunning)
            {
                _totalVerticalOffset = VerticalOffset;
                CurrentVerticalOffset = VerticalOffset;
            }

            _totalVerticalOffset = Math.Min(Math.Max(0, _totalVerticalOffset - e.Delta), ScrollableHeight);
            ScrollToVerticalOffsetWithAnimation(_totalVerticalOffset);
        }
        else
        {
            # 如果滚动方向为水平，则进行水平平滑滚动处理
            if (!_isRunning)
            {
                _totalHorizontalOffset = HorizontalOffset;
                CurrentHorizontalOffset = HorizontalOffset;
            }

            _totalHorizontalOffset = Math.Min(Math.Max(0, _totalHorizontalOffset - e.Delta), ScrollableWidth);
            ScrollToHorizontalOffsetWithAnimation(_totalHorizontalOffset);
        }
    }

    # 定义内部方法，用于平滑滚动到顶部
    internal void ScrollToTopInternal(double milliseconds = 500)
    # 如果当前没有正在运行的动画
    {
        # 设置总垂直偏移量为指定的垂直偏移量
        if (!_isRunning)
        {
            _totalVerticalOffset = VerticalOffset;
            CurrentVerticalOffset = VerticalOffset;
        }

        # 启动一个垂直偏移量为 0 的动画，动画持续时间为 milliseconds 毫秒
        ScrollToVerticalOffsetWithAnimation(0, milliseconds);
    }

    # 启动一个垂直滚动动画
    public void ScrollToVerticalOffsetWithAnimation(double offset, double milliseconds = 500)
    {
        # 创建一个双精度浮点数动画，目标值为 offset，持续时间为 milliseconds
        DoubleAnimation animation = AnimationHelper.CreateAnimation(offset, milliseconds);
        # 设置动画的缓动函数为立方缓动（EaseOut）
        animation.EasingFunction = new CubicEase
        {
            EasingMode = EasingMode.EaseOut
        };
        # 动画结束时不保留最后状态
        animation.FillBehavior = FillBehavior.Stop;
        # 动画完成时设置当前垂直偏移量，并将 _isRunning 标记为 false
        animation.Completed += (s, e1) =>
        {
            CurrentVerticalOffset = offset;
            _isRunning = false;
        };
        # 设置动画为运行状态
        _isRunning = true;

        # 开始动画，动画应用于 CurrentVerticalOffsetProperty 属性
        BeginAnimation(CurrentVerticalOffsetProperty, animation, HandoffBehavior.Compose);
    }

    # 启动一个水平滚动动画
    public void ScrollToHorizontalOffsetWithAnimation(double offset, double milliseconds = 500)
    {
        # 创建一个双精度浮点数动画，目标值为 offset，持续时间为 milliseconds
        DoubleAnimation animation = AnimationHelper.CreateAnimation(offset, milliseconds);
        # 设置动画的缓动函数为立方缓动（EaseOut）
        animation.EasingFunction = new CubicEase
        {
            EasingMode = EasingMode.EaseOut
        };
        # 动画结束时不保留最后状态
        animation.FillBehavior = FillBehavior.Stop;
        # 动画完成时设置当前水平偏移量，并将 _isRunning 标记为 false
        animation.Completed += (s, e1) =>
        {
            CurrentHorizontalOffset = offset;
            _isRunning = false;
        };
        # 设置动画为运行状态
        _isRunning = true;

        # 开始动画，动画应用于 CurrentHorizontalOffsetProperty 属性
        BeginAnimation(CurrentHorizontalOffsetProperty, animation, HandoffBehavior.Compose);
    }

    # 执行击测试时核心逻辑，判断是否需要处理击测试
    protected override HitTestResult? HitTestCore(PointHitTestParameters hitTestParameters)
    {
        # 如果 IsPenetrating 为 true，则返回 null，不进行击测试；否则调用基类的 HitTestCore 方法
        return IsPenetrating ? null : base.HitTestCore(hitTestParameters);
    }

    # 设置是否启用惯性
    public static void SetIsInertiaEnabled(DependencyObject element, bool value)
    {
        element.SetValue(IsInertiaEnabledProperty, value);
    }

    # 获取是否启用惯性
    public static bool GetIsInertiaEnabled(DependencyObject element)
    {
        return (bool)element.GetValue(IsInertiaEnabledProperty);
    }

    # 属性：是否启用惯性
    public bool IsInertiaEnabled
    {
        get => (bool)GetValue(IsInertiaEnabledProperty);
        set => SetValue(IsInertiaEnabledProperty, value);
    }

    # 定义一个附加属性：IsInertiaEnabled，默认值为 true
    public static readonly DependencyProperty IsInertiaEnabledProperty = DependencyProperty.RegisterAttached(nameof(IsInertiaEnabled), typeof(bool), typeof(SmoothScrollViewer), new PropertyMetadata(true));

    # 属性：是否穿透
    public bool IsPenetrating
    {
        get => (bool)GetValue(IsPenetratingProperty);
        set => SetValue(IsPenetratingProperty, value);
    }

    # 设置是否穿透
    public static void SetIsPenetrating(DependencyObject element, bool value)
    {
        element.SetValue(IsPenetratingProperty, value);
    }

    # 获取是否穿透
    public static bool GetIsPenetrating(DependencyObject element)
    {
        return (bool)element.GetValue(IsPenetratingProperty);
    }

    # 定义一个附加属性：IsPenetrating，默认值为 false
    public static readonly DependencyProperty IsPenetratingProperty = DependencyProperty.RegisterAttached(nameof(IsPenetrating), typeof(bool), typeof(SmoothScrollViewer), new PropertyMetadata(false));
    # 当前垂直偏移量的属性，具有 get 和 set 访问器
    internal double CurrentVerticalOffset
    {
        # 获取当前属性值，强制转换为 double 类型
        get => (double)GetValue(CurrentVerticalOffsetProperty);
        # 设置当前属性值
        set => SetValue(CurrentVerticalOffsetProperty, value);
    }

    # 静态属性，定义 CurrentVerticalOffset 的 DependencyProperty
    internal static readonly DependencyProperty CurrentVerticalOffsetProperty = DependencyProperty.Register(nameof(CurrentVerticalOffset), typeof(double), typeof(SmoothScrollViewer), new PropertyMetadata(0d, OnCurrentVerticalOffsetChanged));

    # 当前垂直偏移量更改的回调方法
    private static void OnCurrentVerticalOffsetChanged(DependencyObject d, DependencyPropertyChangedEventArgs e)
    {
        # 如果依赖对象是 SmoothScrollViewer 且新值是 double 类型
        if (d is SmoothScrollViewer ctl && e.NewValue is double v)
        {
            # 调用 SmoothScrollViewer 的 ScrollToVerticalOffset 方法
            ctl.ScrollToVerticalOffset(v);
        }
    }

    # 当前水平偏移量的属性，具有 get 和 set 访问器
    internal double CurrentHorizontalOffset
    {
        # 获取当前属性值，强制转换为 double 类型
        get => (double)GetValue(CurrentHorizontalOffsetProperty);
        # 设置当前属性值
        set => SetValue(CurrentHorizontalOffsetProperty, value);
    }

    # 静态属性，定义 CurrentHorizontalOffset 的 DependencyProperty
    internal static readonly DependencyProperty CurrentHorizontalOffsetProperty = DependencyProperty.Register(nameof(CurrentHorizontalOffset), typeof(double), typeof(SmoothScrollViewer), new PropertyMetadata(0d, OnCurrentHorizontalOffsetChanged));

    # 当前水平偏移量更改的回调方法
    private static void OnCurrentHorizontalOffsetChanged(DependencyObject d, DependencyPropertyChangedEventArgs e)
    {
        # 如果依赖对象是 SmoothScrollViewer 且新值是 double 类型
        if (d is SmoothScrollViewer ctl && e.NewValue is double v)
        {
            # 调用 SmoothScrollViewer 的 ScrollToHorizontalOffset 方法
            ctl.ScrollToHorizontalOffset(v);
        }
    }
# 定义一个封闭的类 AnimationHelper
file sealed class AnimationHelper
{
    # 创建一个静态方法 CreateAnimation，接受目标值和持续时间（默认为 200 毫秒）
    public static DoubleAnimation CreateAnimation(double toValue, double milliseconds = 200)
    {
        # 创建一个新的 DoubleAnimation 对象，目标值为 toValue，持续时间由 milliseconds 转换而来
        return new(toValue, new Duration(TimeSpan.FromMilliseconds(milliseconds)))
        {
            # 设置动画的缓动函数为 PowerEase，并设置缓动模式为 EaseInOut
            EasingFunction = new PowerEase { EasingMode = EasingMode.EaseInOut }
        };
    }
}
```