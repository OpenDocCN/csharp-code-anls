# `.\better-genshin-impact\Build\MicaSetup\Design\Converters\Abstraction\ValueConverterGroup.cs`

```cs
# 引入必要的系统和 WPF 相关命名空间
﻿using System;
using System.Collections.Generic;
using System.Globalization;
using System.Linq;
using System.Windows.Data;
using System.Windows.Markup;

# 定义命名空间 MicaSetup.Design.Converters
namespace MicaSetup.Design.Converters;

# 指定 XAML 内容属性为 Converters 属性
[ContentProperty(nameof(Converters))]
# 指定值转换器的输入和输出类型
[ValueConversion(typeof(object), typeof(object))]
# 定义一个值转换器组类，继承自 SingletonConverterBase
public class ValueConverterGroup : SingletonConverterBase<ValueConverterGroup>
{
    # 定义一个可设置的转换器列表属性
    public List<IValueConverter> Converters { get; set; } = new List<IValueConverter>();

    # 重写 Convert 方法，处理值转换
    protected override object Convert(object value, Type targetType, object parameter, CultureInfo culture)
    {
        # 如果 Converters 是 IEnumerable<IValueConverter> 类型的集合
        if (this.Converters is IEnumerable<IValueConverter> converters)
        {
            var language = culture;
            # 使用 Aggregate 方法依次应用所有转换器
            return converters.Aggregate(value, (current, converter) => converter.Convert(current, targetType, parameter, language));
        }

        # 如果没有转换器，返回未设置值
        return UnsetValue;
    }

    # 重写 ConvertBack 方法，处理值转换回
    protected override object ConvertBack(object value, Type targetType, object parameter, CultureInfo culture)
    {
        # 如果 Converters 是 IEnumerable<IValueConverter> 类型的集合
        if (this.Converters is IEnumerable<IValueConverter> converters)
        {
            var language = culture;
            # 使用 Aggregate 方法逆序应用所有转换器
            return converters.Reverse<IValueConverter>().Aggregate(value, (current, converter) => converter.Convert(current, targetType, parameter, language));
        }

        # 如果没有转换器，返回未设置值
        return UnsetValue;
    }
}
```