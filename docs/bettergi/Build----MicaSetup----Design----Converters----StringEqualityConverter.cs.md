# `.\better-genshin-impact\Build\MicaSetup\Design\Converters\StringEqualityConverter.cs`

```cs
# 使用 System 命名空间中的功能
﻿using System;
# 使用 System.Globalization 命名空间中的功能
using System.Globalization;
# 使用 System.Windows.Data 命名空间中的功能
using System.Windows.Data;

# 声明命名空间
namespace MicaSetup.Design.Converters;

# 指定此转换器用于将 string 类型转换为 bool 类型
[ValueConversion(typeof(string), typeof(bool))]
# 定义一个封闭的转换器类，继承自 SingletonConverterBase
public sealed class StringEqualityConverter : SingletonConverterBase<StringEqualityConverter>
{
    # 重写 Convert 方法，将对象值转换为目标类型
    protected override object Convert(object value, Type targetType, object parameter, CultureInfo culture)
    {
        # 如果 value 是 string 类型
        if (value is string inputString)
        {
            # 返回输入字符串是否等于参数字符串
            return inputString == parameter as string;
        }
        # 如果 value 不是 string 类型，直接返回原值
        return value;
    }

    # 重写 ConvertBack 方法，将目标值转换回原始类型
    protected override object ConvertBack(object value, Type targetType, object parameter, CultureInfo culture)
    {
        # 将值转换为 bool 类型
        bool isChecked = (bool)value;

        # 如果 isChecked 为 false，返回空字符串
        if (!isChecked)
        {
            return string.Empty;
        }
        # 如果 isChecked 为 true，返回参数字符串，若参数为空则返回空字符串
        return (parameter as string) ?? string.Empty;
    }
}
```