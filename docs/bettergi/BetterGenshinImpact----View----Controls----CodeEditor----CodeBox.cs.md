# `.\better-genshin-impact\BetterGenshinImpact\View\Controls\CodeEditor\CodeBox.cs`

```cs
# 引入 AvalonEdit 库的命名空间
﻿using ICSharpCode.AvalonEdit;
# 引入 WPF 相关命名空间
using System.Windows;
using System.Windows.Controls;
using System.Windows.Media;

# 定义命名空间 BetterGenshinImpact.View.Controls.CodeEditor
namespace BetterGenshinImpact.View.Controls.CodeEditor;

# 定义 CodeBox 类，继承自 TextEditor
public class CodeBox : TextEditor
{
    # 定义 Code 属性，用于获取或设置文本内容
    public string Code
    {
        get => Text;  # 获取 TextEditor 的文本
        set => Text = value;  # 设置 TextEditor 的文本
    }

    # 定义 CodeProperty 作为 Code 属性的依赖属性
    public static readonly DependencyProperty CodeProperty =
        DependencyProperty.Register(nameof(Code), typeof(string), typeof(CodeBox), new PropertyMetadata(string.Empty, OnTextChange));

    # 依赖属性值变化时的回调函数
    private static void OnTextChange(DependencyObject d, DependencyPropertyChangedEventArgs e)
    {
        # 检查依赖对象是否为 TextEditor 类型
        if (d is TextEditor editor)
        {
            # 更新 TextEditor 的文本
            editor.Text = (e.NewValue as string)!;
        }
    }

    # 定义 LineWrap 属性，用于设置或获取自动换行
    public bool LineWrap
    {
        get => WordWrap;  # 获取当前 WordWrap 属性值
        set
        {
            if (value)  # 如果设置为 true
            {
                WordWrap = true;  # 启用自动换行
                HorizontalScrollBarVisibility = ScrollBarVisibility.Disabled;  # 禁用水平滚动条
            }
            else  # 如果设置为 false
            {
                WordWrap = false;  # 禁用自动换行
                HorizontalScrollBarVisibility = ScrollBarVisibility.Auto;  # 启用水平滚动条
            }
        }
    }

    # CodeBox 类的构造函数
    public CodeBox()
    {
        # 显示行号
        ShowLineNumbers = true;
        # 设置文本区选中区域的刷色
        TextArea.SelectionBrush = new SolidColorBrush(Color.FromRgb(0x26, 0x4F, 0x78));
        # 设置选中区域边框
        TextArea.SelectionBorder = new(Brushes.Transparent, 0d);
        # 设置选中区域的圆角半径
        TextArea.SelectionCornerRadius = 2d;
        # 设置选中区域前景色
        TextArea.SelectionForeground = null!;
    }
}
```