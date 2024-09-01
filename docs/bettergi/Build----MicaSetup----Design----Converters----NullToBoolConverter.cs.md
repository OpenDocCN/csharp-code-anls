# `.\better-genshin-impact\Build\MicaSetup\Design\Converters\NullToBoolConverter.cs`

```cs
# 命名空间引入，用于引入系统、文化、窗口和数据绑定相关的类
﻿using System;
using System.Globalization;
using System.Windows;
using System.Windows.Data;

# 指定命名空间
namespace MicaSetup.Design.Converters;

# 指定值转换器，转换源类型为 object，目标类型为 bool
[ValueConversion(typeof(object), typeof(bool))]
# 定义 NullToBoolConverter 类，继承自 SingletonConverterBase<NullToBoolConverter>
public class NullToBoolConverter : SingletonConverterBase<NullToBoolConverter>
{
    # 注册 DependencyProperty，以便在 XAML 中使用
    public static readonly DependencyProperty IsInvertedProperty = DependencyProperty.Register(
        nameof(IsInverted),               # 依赖属性名称
        typeof(bool),                     # 依赖属性类型
        typeof(NullToBoolConverter),      # 所属类型
        new PropertyMetadata(false));     # 默认值

    # IsInverted 属性，表示是否反转转换逻辑
    public bool IsInverted
    {
        get { return (bool)this.GetValue(IsInvertedProperty); }  # 获取 IsInverted 的值
        set { this.SetValue(IsInvertedProperty, value); }        # 设置 IsInverted 的值
    }

    # 重写 Convert 方法，实现具体的转换逻辑
    protected override object Convert(object value, Type targetType, object parameter, CultureInfo culture)
    {
        # 如果值为 null，则返回反转后的结果；否则返回值为 null 的反转结果
        return value == null ^ this.IsInverted;
    }
}
```