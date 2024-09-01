# `.\better-genshin-impact\Build\MicaSetup\Design\Converters\StringIsNotNullOrEmptyConverter.cs`

```cs
# 导入必要的命名空间
﻿using System;
using System.Globalization;
using System.Windows;
using System.Windows.Data;

# 命名空间声明
namespace MicaSetup.Design.Converters;

# 声明一个值转换器，用于将 string 类型转换为 bool 类型
[ValueConversion(typeof(string), typeof(bool))]
public class StringIsNotNullOrEmptyConverter : StringIsNullOrEmptyConverter
{
    # 构造函数，设置 IsInverted 属性为 true
    public StringIsNotNullOrEmptyConverter()
    {
        this.IsInverted = true;
    }
}

# 声明一个值转换器，用于将 string 类型转换为 bool 类型
[ValueConversion(typeof(string), typeof(bool))]
public class StringIsNullOrEmptyConverter : SingletonConverterBase<StringIsNotNullOrEmptyConverter>
{
    # 注册一个依赖属性 IsInverted，用于控制转换逻辑的反转
    public static readonly DependencyProperty IsInvertedProperty = DependencyProperty.Register(
        nameof(IsInverted),
        typeof(bool),
        typeof(StringIsNullOrEmptyConverter),
        new PropertyMetadata(false));

    # IsInverted 属性的封装器，获取或设置该属性的值
    public bool IsInverted
    {
        get { return (bool)this.GetValue(IsInvertedProperty); }
        set { this.SetValue(IsInvertedProperty, value); }
    }

    # 转换方法，根据 IsInverted 属性决定是否反转逻辑
    protected override object Convert(object value, Type targetType, object parameter, CultureInfo culture)
    {
        # 如果目标类型为 null，抛出异常
        if (targetType is null)
        {
            throw new ArgumentNullException(nameof(targetType));
        }

        # 根据 IsInverted 属性决定返回值的逻辑
        if (this.IsInverted)
        {
            return !string.IsNullOrEmpty(value as string);
        }

        return string.IsNullOrEmpty(value as string);
    }
}
```