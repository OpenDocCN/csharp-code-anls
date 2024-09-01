# `.\better-genshin-impact\Build\MicaSetup\Design\Controls\TextBox\TextBox.cs`

```cs
using CommunityToolkit.Mvvm.Input;  // 引入 CommunityToolkit 中的命令相关功能
using System.Diagnostics;  // 引入用于调试的功能
using System.Windows;  // 引入 WPF 的基础功能
using System.Windows.Controls;  // 引入 WPF 控件基础功能

namespace MicaSetup.Design.Controls;  // 定义命名空间

public class TextBoxEx : TextBox  // 定义一个继承自 TextBox 的自定义控件类
{
    // 注册一个名为 PlaceholderText 的依赖属性
    public static readonly DependencyProperty PlaceholderTextProperty = DependencyProperty.Register(
        nameof(PlaceholderText),  // 属性名称
        typeof(string),  // 属性类型
        typeof(TextBoxEx),  // 属性所属类型
        new PropertyMetadata(string.Empty)  // 默认值
    );

    // 注册一个名为 PlaceholderEnabled 的依赖属性
    public static readonly DependencyProperty PlaceholderEnabledProperty = DependencyProperty.Register(
        nameof(PlaceholderEnabled),  // 属性名称
        typeof(bool),  // 属性类型
        typeof(TextBoxEx),  // 属性所属类型
        new PropertyMetadata(true)  // 默认值
    );

    // 注册一个名为 IsTextSelectionEnabled 的依赖属性
    public static readonly DependencyProperty IsTextSelectionEnabledProperty = DependencyProperty.Register(
        nameof(IsTextSelectionEnabled),  // 属性名称
        typeof(bool),  // 属性类型
        typeof(TextBoxEx),  // 属性所属类型
        new PropertyMetadata(false)  // 默认值
    );

    // 注册一个名为 TemplateButtonCommand 的依赖属性
    public static readonly DependencyProperty TemplateButtonCommandProperty = DependencyProperty.Register(
        nameof(TemplateButtonCommand),  // 属性名称
        typeof(IRelayCommand),  // 属性类型
        typeof(TextBoxEx),  // 属性所属类型
        new PropertyMetadata(null)  // 默认值
    );

    // 定义 PlaceholderText 属性的 getter 和 setter
    public string PlaceholderText
    {
        get => (string)GetValue(PlaceholderTextProperty);  // 获取属性值
        set => SetValue(PlaceholderTextProperty, value);  // 设置属性值
    }

    // 定义 PlaceholderEnabled 属性的 getter 和 setter
    public bool PlaceholderEnabled
    {
        get => (bool)GetValue(PlaceholderEnabledProperty);  // 获取属性值
        set => SetValue(PlaceholderEnabledProperty, value);  // 设置属性值
    }

    /// <summary>
    /// TODO
    /// </summary>
    // 定义 IsTextSelectionEnabled 属性的 getter 和 setter
    public bool IsTextSelectionEnabled
    {
        get => (bool)GetValue(IsTextSelectionEnabledProperty);  // 获取属性值
        set => SetValue(IsTextSelectionEnabledProperty, value);  // 设置属性值
    }

    // 定义 TemplateButtonCommand 属性的 getter
    public IRelayCommand TemplateButtonCommand => (IRelayCommand)GetValue(TemplateButtonCommandProperty);  // 获取属性值

    // 构造函数
    public TextBoxEx()
    {
        // 设置 TemplateButtonCommand 属性的默认值为一个新的 RelayCommand 实例
        SetValue(TemplateButtonCommandProperty, new RelayCommand<string>(OnTemplateButtonClick));
    }

    // 重写 OnTextChanged 方法，处理文本变化事件
    protected override void OnTextChanged(TextChangedEventArgs e)
    {
        base.OnTextChanged(e);  // 调用基类的方法

        // 如果 PlaceholderEnabled 为 true 且文本长度大于 0，则将 PlaceholderEnabled 设为 false
        if (PlaceholderEnabled && Text.Length > 0)
            PlaceholderEnabled = false;

        // 如果 PlaceholderEnabled 为 false 且文本长度小于 1，则将 PlaceholderEnabled 设为 true
        if (!PlaceholderEnabled && Text.Length < 1)
            PlaceholderEnabled = true;
    }

    // 重写 OnGotFocus 方法，处理获得焦点事件
    protected override void OnGotFocus(RoutedEventArgs e)
    {
        base.OnGotFocus(e);  // 调用基类的方法

        // 将光标位置设置为文本的末尾
        CaretIndex = Text.Length;
    }

    // 重写 OnLostFocus 方法，处理失去焦点事件
    protected override void OnLostFocus(RoutedEventArgs e)
    {
        base.OnLostFocus(e);  // 调用基类的方法
    }

    // 定义一个虚方法，用于处理清除按钮点击事件
    protected virtual void OnClearButtonClick()
    {
        // 如果文本长度大于 0，则将文本清空
        if (Text.Length > 0)
        {
            Text = string.Empty;
        }
    }

    // 定义一个虚方法，用于处理模板按钮点击事件
    protected virtual void OnTemplateButtonClick(string? parameter)
    {
        // 输出调试信息，表示按钮被点击
        Debug.WriteLine($"INFO: {typeof(TextBoxEx)} button clicked");
        // 调用 OnClearButtonClick 方法
        OnClearButtonClick();
    }
}
```