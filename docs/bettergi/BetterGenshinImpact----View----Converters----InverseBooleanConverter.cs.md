# `.\better-genshin-impact\BetterGenshinImpact\View\Converters\InverseBooleanConverter.cs`

```cs
# 定义命名空间并包含所需的库
﻿using System;
using System.Windows;
using System.Windows.Data;

namespace BetterGenshinImpact.View.Converters;

# 指定该类为值转换器，将布尔值转换为布尔值的相反值
[ValueConversion(typeof(bool), typeof(bool))]
public sealed class InverseBooleanConverter : IValueConverter
{
    # 实现 IValueConverter 接口的 Convert 方法
    public object Convert(object? value, Type targetType, object? parameter, System.Globalization.CultureInfo culture)
    {
        # 检查目标类型是否为布尔值
        if (targetType != typeof(bool))
            throw new InvalidOperationException("The target must be a boolean");
        # 如果值为 null，则返回未设置的依赖属性值
        if (value == null)
            return DependencyProperty.UnsetValue;
        # 返回布尔值的相反值
        return !(bool)value;
    }

    # 实现 IValueConverter 接口的 ConvertBack 方法
    public object ConvertBack(object? value, Type targetType, object? parameter, System.Globalization.CultureInfo culture)
    {
        # 由于此转换器不支持反向转换，抛出异常
        throw new NotSupportedException();
    }
}
```