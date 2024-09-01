# `.\better-genshin-impact\BetterGenshinImpact\View\Converters\BooleanToVisibilityRevertConverter.cs`

```cs
# 引入必要的系统和 Windows 命名空间
﻿using System;
using System.Windows;
using System.Windows.Data;

# 定义命名空间，包含转换器类
namespace BetterGenshinImpact.View.Converters;

# 定义一个用于将布尔值转换为可见性反转的转换器类
[ValueConversion(typeof(bool), typeof(Visibility))]
public sealed class BooleanToVisibilityRevertConverter : IValueConverter
{
    # 将布尔值转换为反转的可见性
    public object Convert(object value, Type targetType, object parameter, System.Globalization.CultureInfo culture)
    {
        # 如果值是布尔类型并且为 true，返回 Collapsed，否则返回 Visible
        return (value is bool v && v) ? Visibility.Collapsed : Visibility.Visible;
    }

    # 将可见性转换回布尔值
    public object ConvertBack(object value, Type targetType, object parameter, System.Globalization.CultureInfo culture)
    {
        # 如果值是 Collapsed，返回 true，否则返回 false
        return value is Visibility.Collapsed;
    }
}
```