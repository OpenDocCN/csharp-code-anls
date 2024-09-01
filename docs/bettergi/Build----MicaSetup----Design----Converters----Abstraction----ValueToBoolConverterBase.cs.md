# `.\better-genshin-impact\Build\MicaSetup\Design\Converters\Abstraction\ValueToBoolConverterBase.cs`

```cs
﻿using System;
using System.Globalization;
using Property = System.Windows.DependencyProperty;

namespace MicaSetup.Design.Converters;

# 定义一个抽象基类，用于将某个值转换为布尔值
public abstract class ValueToBoolConverterBase<T, TConverter> : ConverterBase
    where TConverter : new()
{
    # 定义一个抽象属性，用于表示布尔转换中的“真”值
    public abstract T TrueValue { get; set; }

    # 定义一个布尔属性，用于指示是否需要反转转换结果
    public bool IsInverted
    {
        get { return (bool)this.GetValue(IsInvertedProperty); }  # 获取当前的“反转”状态
        set { this.SetValue(IsInvertedProperty, value); }  # 设置“反转”状态
    }

    # 重写基类方法，用于执行值到布尔值的转换
    protected override object Convert(object value, Type targetType, object parameter, CultureInfo culture)
    {
        var trueValue = this.TrueValue;  # 获取“真”值
        return Equals(value, trueValue) ^ this.IsInverted;  # 比较值是否等于“真”值，并应用反转标志
    }

    # 定义静态字段，用于表示“反转”属性
    public static readonly Property IsInvertedProperty = PropertyHelper.Create<bool, ValueToBoolConverterBase<T, TConverter>>(nameof(IsInverted));
}
```