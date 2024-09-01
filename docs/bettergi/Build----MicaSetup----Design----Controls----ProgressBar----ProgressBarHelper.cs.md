# `.\better-genshin-impact\Build\MicaSetup\Design\Controls\ProgressBar\ProgressBarHelper.cs`

```cs
# 引入系统窗体和绘制相关的命名空间
﻿using System.Windows;
using System.Windows.Media;

# 定义一个用于帮助设置进度条前景色的辅助类
namespace MicaSetup.Design.Controls;

public class ProgressBarHelper
{
    # 获取附加的前景色属性1的值
    public static Color GetForeground1(DependencyObject obj)
    {
        return (Color)obj.GetValue(Foreground1Property);
    }

    # 设置附加的前景色属性1的值
    public static void SetForeground1(DependencyObject obj, Color value)
    {
        obj.SetValue(Foreground1Property, value);
    }

    # 定义附加的前景色属性1，初始值为颜色#73EBF3
    public static readonly DependencyProperty Foreground1Property = DependencyProperty.RegisterAttached("Foreground1", typeof(Color), typeof(ProgressBarHelper), new((Color)ColorConverter.ConvertFromString("#73EBF3")));

    # 获取附加的前景色属性2的值
    public static Color GetForeground2(DependencyObject obj)
    {
        return (Color)obj.GetValue(Foreground2Property);
    }

    # 设置附加的前景色属性2的值
    public static void SetForeground2(DependencyObject obj, Color value)
    {
        obj.SetValue(Foreground2Property, value);
    }

    # 定义附加的前景色属性2，初始值为颜色#238EFA
    public static readonly DependencyProperty Foreground2Property = DependencyProperty.RegisterAttached("Foreground2", typeof(Color), typeof(ProgressBarHelper), new((Color)ColorConverter.ConvertFromString("#238EFA")));
}
```