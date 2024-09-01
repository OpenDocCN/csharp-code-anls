# `.\better-genshin-impact\Build\MicaSetup\Design\Controls\BitmapIcon\BitmapIcon.cs`

```cs
# 引入系统相关命名空间
﻿using System;
using System.Windows;
using System.Windows.Controls;
using System.Windows.Media;
using System.Windows.Media.Imaging;
using System.Windows.Shapes;

namespace MicaSetup.Design.Controls;

# 禁用特定的编译器警告 CS8618，表示未初始化的非 nullable 属性
#pragma warning disable CS8618

# 定义 BitmapIcon 类，继承自 IconElement
public class BitmapIcon : IconElement
{
    # 静态构造函数，用于初始化静态成员
    static BitmapIcon()
    {
        # 覆盖 BitmapIcon 类的 ForegroundProperty 元数据，设置 Foreground 属性改变时的回调方法
        ForegroundProperty.OverrideMetadata(typeof(BitmapIcon), new FrameworkPropertyMetadata(OnForegroundChanged));
    }

    # 默认构造函数
    public BitmapIcon()
    {
    }

    # 定义 UriSource 依赖属性，用于指定图像的 URI 来源
    public static readonly DependencyProperty UriSourceProperty =
        BitmapImage.UriSourceProperty.AddOwner(
            typeof(BitmapIcon),
            new FrameworkPropertyMetadata(OnUriSourceChanged));

    # UriSource 属性的封装，允许外部访问和设置图像 URI 来源
    public Uri UriSource
    {
        get => (Uri)GetValue(UriSourceProperty);
        set => SetValue(UriSourceProperty, value);
    }

    # 当 UriSource 依赖属性的值发生变化时调用的静态方法
    private static void OnUriSourceChanged(DependencyObject d, DependencyPropertyChangedEventArgs e)
    {
        # 调用实例方法应用新的 URI 来源
        ((BitmapIcon)d).ApplyUriSource();
    }

    # 定义 ShowAsMonochrome 依赖属性，用于指定图标是否以单色显示
    public static readonly DependencyProperty ShowAsMonochromeProperty =
        DependencyProperty.Register(
            nameof(ShowAsMonochrome),
            typeof(bool),
            typeof(BitmapIcon),
            new PropertyMetadata(true, OnShowAsMonochromeChanged));

    # ShowAsMonochrome 属性的封装，允许外部访问和设置图标显示模式
    public bool ShowAsMonochrome
    {
        get => (bool)GetValue(ShowAsMonochromeProperty);
        set => SetValue(ShowAsMonochromeProperty, value);
    }

    # 当 ShowAsMonochrome 依赖属性的值发生变化时调用的静态方法
    private static void OnShowAsMonochromeChanged(DependencyObject d, DependencyPropertyChangedEventArgs e)
    {
        # 调用实例方法应用新的显示模式
        ((BitmapIcon)d).ApplyShowAsMonochrome();
    }

    # 初始化子元素的方法，设置图标的显示和样式
    private protected override void InitializeChildren()
    {
        # 创建并初始化 Image 控件，初始时设置为隐藏
        _image = new Image
        {
            Visibility = Visibility.Hidden
        };

        # 创建并初始化 ImageBrush 控件用于处理前景图像的透明度
        _opacityMask = new ImageBrush();
        # 创建并初始化 Rectangle 控件，设置其 OpacityMask 为上面创建的 ImageBrush
        _foreground = new Rectangle
        {
            OpacityMask = _opacityMask
        };

        # 应用前景设置
        ApplyForeground();
        # 应用 URI 来源设置
        ApplyUriSource();

        # 将 Image 控件添加到子控件集合中
        Children.Add(_image);

        # 应用单色显示设置
        ApplyShowAsMonochrome();
    }

    # 当前景属性继承设置发生变化时调用的方法
    private protected override void OnShouldInheritForegroundFromVisualParentChanged()
    {
        # 应用前景设置
        ApplyForeground();
    }

    # 当视觉父控件的前景属性发生变化时调用的方法
    private protected override void OnVisualParentForegroundPropertyChanged(DependencyPropertyChangedEventArgs args)
    {
        # 如果当前控件需要从视觉父控件继承前景属性，则应用前景设置
        if (ShouldInheritForegroundFromVisualParent)
        {
            ApplyForeground();
        }
    }

    # 当 Foreground 依赖属性的值发生变化时调用的静态方法
    private static void OnForegroundChanged(DependencyObject d, DependencyPropertyChangedEventArgs e)
    {
        # 调用实例方法应用新的前景
        ((BitmapIcon)d).ApplyForeground();
    }

    # 应用前景设置的方法
    private void ApplyForeground()
    {
        # 如果 _foreground 控件不为空，则设置其 Fill 属性
        if (_foreground != null)
        {
            _foreground.Fill = ShouldInheritForegroundFromVisualParent ? VisualParentForeground : Foreground;
        }
    }

    # 应用 URI 来源设置的方法
    private void ApplyUriSource()
    # 检查 _image 和 _opacityMask 是否都不为空
    {
        if (_image != null && _opacityMask != null)
        {
            # 获取 UriSource
            var uriSource = UriSource;
            # 如果 UriSource 不为空
            if (uriSource != null)
            {
                # 使用 uriSource 创建 BitmapImage 对象
                var imageSource = new BitmapImage(uriSource);
                # 设置 _image 的 Source 属性为 imageSource
                _image.Source = imageSource;
                # 设置 _opacityMask 的 ImageSource 属性为 imageSource
                _opacityMask.ImageSource = imageSource;
            }
            else
            {
                # 如果 uriSource 为空，清除 _image 的 Source 属性
                _image.ClearValue(Image.SourceProperty);
                # 如果 uriSource 为空，清除 _opacityMask 的 ImageSource 属性
                _opacityMask.ClearValue(ImageBrush.ImageSourceProperty);
            }
        }
    }

    # 将图像应用为单色模式
    private void ApplyShowAsMonochrome()
    {
        # 获取 ShowAsMonochrome 属性的值
        bool showAsMonochrome = ShowAsMonochrome;

        # 如果 _image 不为空
        if (_image != null)
        {
            # 根据 showAsMonochrome 值设置 _image 的可见性
            _image.Visibility = showAsMonochrome ? Visibility.Hidden : Visibility.Visible;
        }

        # 如果 _foreground 不为空
        if (_foreground != null)
        {
            # 根据 showAsMonochrome 值处理 _foreground 的显示
            if (showAsMonochrome)
            {
                # 如果 _foreground 不在 Children 集合中，则添加它
                if (!Children.Contains(_foreground))
                {
                    Children.Add(_foreground);
                }
            }
            else
            {
                # 否则，从 Children 集合中移除 _foreground
                Children.Remove(_foreground);
            }
        }
    }

    # 声明图像、前景矩形和透明度遮罩
    private Image _image;
    private Rectangle _foreground;
    private ImageBrush _opacityMask;
你可以提供一段具体的代码来注释吗？这样我可以更准确地帮助你。
```