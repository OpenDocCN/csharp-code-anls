# `.\better-genshin-impact\Build\MicaSetup\Design\Converters\IsEmptyConverter.cs`

```cs
# 引入必要的命名空间
﻿using System;
using System.Collections;
using System.Globalization;
using System.Windows;
using System.Windows.Data;

# 声明命名空间
namespace MicaSetup.Design.Converters;

# 定义将 IEnumerable 或 object 转换为 bool 的值转换器
[ValueConversion(typeof(IEnumerable), typeof(bool))]
[ValueConversion(typeof(object), typeof(bool))]
public class IsEmptyConverter : SingletonConverterBase<IsEmptyConverter>
{
    # 注册 IsInverted 属性作为依赖属性
    public static readonly DependencyProperty IsInvertedProperty = DependencyProperty.Register(
        nameof(IsInverted),  # 属性名称
        typeof(bool),  # 属性类型
        typeof(IsEmptyConverter),  # 所属类
        new PropertyMetadata(false));  # 默认值

    # IsInverted 属性的访问器
    public bool IsInverted
    {
        # 获取 IsInverted 属性值
        get => (bool)this.GetValue(IsInvertedProperty);
        # 设置 IsInverted 属性值
        set => this.SetValue(IsInvertedProperty, value);
    }

    # 实现转换逻辑
    protected override object Convert(object value, Type targetType, object parameter, CultureInfo culture)
    {
        # 检查 value 是否是 IEnumerable 类型
        if (value is IEnumerable enumerable)
        {
            # 检查 enumerable 是否至少有一个元素
            var hasAtLeastOne = enumerable.GetEnumerator().MoveNext();
            # 返回是否为空的结果，考虑 IsInverted 属性
            return (hasAtLeastOne == false) ^ this.IsInverted;
        }

        # 对于非 IEnumerable 类型，返回默认值考虑 IsInverted 属性
        return true ^ this.IsInverted;
    }
}
```