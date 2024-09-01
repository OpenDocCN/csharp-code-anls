# `.\better-genshin-impact\Build\MicaSetup\Design\Converters\ValueToBoolConverter.cs`

```cs
# 引入系统窗口数据绑定命名空间
﻿using System.Windows.Data;
# 引入 DependencyProperty 类型的别名
using Property = System.Windows.DependencyProperty;

namespace MicaSetup.Design.Converters;

# 指定值转换器的输入和输出类型
[ValueConversion(typeof(object), typeof(bool))]
# 定义泛型值到布尔值的转换器类
public class ValueToBoolConverter<T> : ReversibleValueToBoolConverterBase<T, ValueToBoolConverter<T>>
{
    # 获取或设置转换器的 TrueValue 属性
    public override T TrueValue
    {
        get => (T)this.GetValue(TrueValueProperty);
        set => this.SetValue(TrueValueProperty, value);
    }

    # 定义 TrueValue 属性的依赖属性
    public static readonly Property TrueValueProperty =
        PropertyHelper.Create<T, ValueToBoolConverter<T>>(nameof(TrueValue));

    # 获取或设置转换器的 FalseValue 属性
    public override T FalseValue
    {
        get => (T)this.GetValue(FalseValueProperty);
        set => this.SetValue(FalseValueProperty, value);
    }

    # 定义 FalseValue 属性的依赖属性
    public static readonly Property FalseValueProperty =
        PropertyHelper.Create<T, ValueToBoolConverter<T>>(nameof(FalseValue));
}

# 指定值转换器的输入和输出类型
[ValueConversion(typeof(object), typeof(bool))]
# 定义不带泛型参数的值到布尔值的转换器类
public class ValueToBoolConverter : ValueToBoolConverter<object>
{
}
```