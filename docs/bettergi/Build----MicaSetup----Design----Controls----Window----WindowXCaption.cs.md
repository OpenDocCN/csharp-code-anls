# `.\better-genshin-impact\Build\MicaSetup\Design\Controls\Window\WindowXCaption.cs`

```cs
# 定义一个名为 WindowXCaption 的类，它包含了一些附加的依赖属性
public sealed class WindowXCaption
{
    # 获取附加依赖属性 Icon 的值
    public static ImageSource GetIcon(DependencyObject obj)
    {
        return (ImageSource)obj.GetValue(IconProperty);
    }

    # 设置附加依赖属性 Icon 的值
    public static void SetIcon(DependencyObject obj, ImageSource value)
    {
        obj.SetValue(IconProperty, value);
    }

    # 定义附加依赖属性 Icon
    public static readonly DependencyProperty IconProperty =
        DependencyProperty.RegisterAttached("Icon", typeof(ImageSource), typeof(WindowXCaption));

    # 获取附加依赖属性 Padding 的值
    public static Thickness GetPadding(DependencyObject obj)
    {
        return (Thickness)obj.GetValue(PaddingProperty);
    }

    # 设置附加依赖属性 Padding 的值
    public static void SetPadding(DependencyObject obj, Thickness value)
    {
        obj.SetValue(PaddingProperty, value);
    }

    # 定义附加依赖属性 Padding
    public static readonly DependencyProperty PaddingProperty =
        DependencyProperty.RegisterAttached("Padding", typeof(Thickness), typeof(WindowXCaption));

    # 获取附加依赖属性 Height 的值
    public static double GetHeight(DependencyObject obj)
    {
        return (double)obj.GetValue(HeightProperty);
    }

    # 设置附加依赖属性 Height 的值
    public static void SetHeight(DependencyObject obj, double value)
    {
        obj.SetValue(HeightProperty, value);
    }

    # 定义附加依赖属性 Height
    public static readonly DependencyProperty HeightProperty =
        DependencyProperty.RegisterAttached("Height", typeof(double), typeof(WindowXCaption));

    # 获取附加依赖属性 Foreground 的值
    public static Brush GetForeground(DependencyObject obj)
    {
        return (Brush)obj.GetValue(ForegroundProperty);
    }

    # 设置附加依赖属性 Foreground 的值
    public static void SetForeground(DependencyObject obj, Brush value)
    {
        obj.SetValue(ForegroundProperty, value);
    }

    # 定义附加依赖属性 Foreground
    public static readonly DependencyProperty ForegroundProperty =
        DependencyProperty.RegisterAttached("Foreground", typeof(Brush), typeof(WindowXCaption));

    # 获取附加依赖属性 Background 的值
    public static Brush GetBackground(DependencyObject obj)
    {
        return (Brush)obj.GetValue(BackgroundProperty);
    }

    # 设置附加依赖属性 Background 的值
    public static void SetBackground(DependencyObject obj, Brush value)
    {
        obj.SetValue(BackgroundProperty, value);
    }

    # 定义附加依赖属性 Background
    public static readonly DependencyProperty BackgroundProperty =
        DependencyProperty.RegisterAttached("Background", typeof(Brush), typeof(WindowXCaption));

    # 获取附加依赖属性 MinimizeButtonStyle 的值
    public static Style GetMinimizeButtonStyle(DependencyObject obj)
    {
        return (Style)obj.GetValue(MinimizeButtonStyleProperty);
    }

    # 设置附加依赖属性 MinimizeButtonStyle 的值
    public static void SetMinimizeButtonStyle(DependencyObject obj, Style value)
    {
        obj.SetValue(MinimizeButtonStyleProperty, value);
    }

    # 定义附加依赖属性 MinimizeButtonStyle
    public static readonly DependencyProperty MinimizeButtonStyleProperty =
        DependencyProperty.RegisterAttached("MinimizeButtonStyle", typeof(Style), typeof(WindowXCaption));

    # 获取附加依赖属性 MaximizeButtonStyle 的值
    public static Style GetMaximizeButtonStyle(DependencyObject obj)
    {
        return (Style)obj.GetValue(MaximizeButtonStyleProperty);
    }

    # 设置附加依赖属性 MaximizeButtonStyle 的值
    public static void SetMaximizeButtonStyle(DependencyObject obj, Style value)
    {
        obj.SetValue(MaximizeButtonStyleProperty, value);
    }

    # 定义附加依赖属性 MaximizeButtonStyle
    public static readonly DependencyProperty MaximizeButtonStyleProperty =
        DependencyProperty.RegisterAttached("MaximizeButtonStyle", typeof(Style), typeof(WindowXCaption));
    # 设置依赖属性 MaximizeButtonStyleProperty 的值
    {
        obj.SetValue(MaximizeButtonStyleProperty, value);
    }

    # 定义一个只读的依赖属性 MaximizeButtonStyleProperty
    public static readonly DependencyProperty MaximizeButtonStyleProperty =
        # 注册一个附加依赖属性 "MaximizeButtonStyle"，其类型为 Style，所属类为 WindowXCaption
        DependencyProperty.RegisterAttached("MaximizeButtonStyle", typeof(Style), typeof(WindowXCaption));

    # 获取指定对象的附加属性 CloseButtonStyle 的值
    public static Style GetCloseButtonStyle(DependencyObject obj)
    {
        return (Style)obj.GetValue(CloseButtonStyleProperty);
    }

    # 设置指定对象的附加属性 CloseButtonStyle 的值
    public static void SetCloseButtonStyle(DependencyObject obj, Style value)
    {
        obj.SetValue(CloseButtonStyleProperty, value);
    }

    # 定义一个只读的依赖属性 CloseButtonStyleProperty
    public static readonly DependencyProperty CloseButtonStyleProperty =
        # 注册一个附加依赖属性 "CloseButtonStyle"，其类型为 Style，所属类为 WindowXCaption
        DependencyProperty.RegisterAttached("CloseButtonStyle", typeof(Style), typeof(WindowXCaption));

    # 获取指定对象的附加属性 FullScreenButtonStyle 的值
    public static Style GetFullScreenButtonStyle(DependencyObject obj)
    {
        return (Style)obj.GetValue(FullScreenButtonStyleProperty);
    }

    # 设置指定对象的附加属性 FullScreenButtonStyle 的值
    public static void SetFullScreenButtonStyle(DependencyObject obj, Style value)
    {
        obj.SetValue(FullScreenButtonStyleProperty, value);
    }

    # 定义一个只读的依赖属性 FullScreenButtonStyleProperty
    public static readonly DependencyProperty FullScreenButtonStyleProperty =
        # 注册一个附加依赖属性 "FullScreenButtonStyle"，其类型为 Style，所属类为 WindowXCaption
        DependencyProperty.RegisterAttached("FullScreenButtonStyle", typeof(Style), typeof(WindowXCaption));

    # 获取指定对象的附加属性 Header 的值
    public static object GetHeader(DependencyObject obj)
    {
        return obj.GetValue(HeaderProperty);
    }

    # 设置指定对象的附加属性 Header 的值
    public static void SetHeader(DependencyObject obj, object value)
    {
        obj.SetValue(HeaderProperty, value);
    }

    # 定义一个只读的依赖属性 HeaderProperty
    public static readonly DependencyProperty HeaderProperty =
        # 注册一个附加依赖属性 "Header"，其类型为 object，所属类为 WindowXCaption
        DependencyProperty.RegisterAttached("Header", typeof(object), typeof(WindowXCaption));

    # 获取指定对象的附加属性 ExtendControl 的值
    public static UIElement GetExtendControl(DependencyObject obj)
    {
        return (UIElement)obj.GetValue(ExtendControlProperty);
    }

    # 设置指定对象的附加属性 ExtendControl 的值
    public static void SetExtendControl(DependencyObject obj, UIElement value)
    {
        obj.SetValue(ExtendControlProperty, value);
    }

    # 定义一个只读的依赖属性 ExtendControlProperty
    public static readonly DependencyProperty ExtendControlProperty =
        # 注册一个附加依赖属性 "ExtendControl"，其类型为 UIElement，所属类为 WindowXCaption
        DependencyProperty.RegisterAttached("ExtendControl", typeof(UIElement), typeof(WindowXCaption));

    # 获取指定对象的附加属性 DisableCloseButton 的值
    public static bool GetDisableCloseButton(DependencyObject obj)
    {
        return (bool)obj.GetValue(DisableCloseButtonProperty);
    }

    # 设置指定对象的附加属性 DisableCloseButton 的值
    public static void SetDisableCloseButton(DependencyObject obj, bool value)
    {
        obj.SetValue(DisableCloseButtonProperty, value);
    }

    # 定义一个只读的依赖属性 DisableCloseButtonProperty
    public static readonly DependencyProperty DisableCloseButtonProperty =
        # 注册一个附加依赖属性 "DisableCloseButton"，其类型为 bool，所属类为 WindowXCaption
        DependencyProperty.RegisterAttached("DisableCloseButton", typeof(bool), typeof(WindowXCaption));

    # 获取指定对象的附加属性 HideBasicButtons 的值
    public static bool GetHideBasicButtons(DependencyObject obj)
    {
        return (bool)obj.GetValue(HideBasicButtonsProperty);
    }

    # 设置指定对象的附加属性 HideBasicButtons 的值
    public static void SetHideBasicButtons(DependencyObject obj, bool value)
    {
        obj.SetValue(HideBasicButtonsProperty, value);
    }
    // 定义一个只读的依赖属性，用于隐藏基本按钮
    public static readonly DependencyProperty HideBasicButtonsProperty =
        // 注册一个附加的依赖属性，类型为 bool，属于 WindowXCaption 类型
        DependencyProperty.RegisterAttached("HideBasicButtons", typeof(bool), typeof(WindowXCaption));

    // 获取依赖对象的全屏按钮显示状态
    public static bool GetShowFullScreenButton(DependencyObject obj)
    {
        // 从依赖对象中获取 ShowFullScreenButtonProperty 属性的值，并转换为 bool 类型
        return (bool)obj.GetValue(ShowFullScreenButtonProperty);
    }

    // 设置依赖对象的全屏按钮显示状态
    public static void SetShowFullScreenButton(DependencyObject obj, bool value)
    {
        // 将全屏按钮显示状态的值设置到依赖对象中
        obj.SetValue(ShowFullScreenButtonProperty, value);
    }

    // 定义一个只读的依赖属性，用于全屏按钮的显示控制
    public static readonly DependencyProperty ShowFullScreenButtonProperty =
        // 注册一个附加的依赖属性，类型为 bool，属于 WindowXCaption 类型，默认值为 false
        DependencyProperty.RegisterAttached("ShowFullScreenButton", typeof(bool), typeof(WindowXCaption), new(false));
# 结束当前代码块或代码块中的控制结构
}
```