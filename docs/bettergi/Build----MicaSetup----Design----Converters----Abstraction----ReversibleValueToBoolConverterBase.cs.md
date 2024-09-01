# `.\better-genshin-impact\Build\MicaSetup\Design\Converters\Abstraction\ReversibleValueToBoolConverterBase.cs`

```cs
﻿using System; // 引入 System 命名空间
using System.Globalization; // 引入 System.Globalization 命名空间以支持文化相关的功能
using Property = System.Windows.DependencyProperty; // 将 System.Windows.DependencyProperty 重命名为 Property 以简化代码书写

namespace MicaSetup.Design.Converters; // 定义命名空间 MicaSetup.Design.Converters

// 定义一个抽象类 ReversibleValueToBoolConverterBase，继承自 ValueToBoolConverterBase
public abstract class ReversibleValueToBoolConverterBase<T, TConverter> : ValueToBoolConverterBase<T, TConverter>
    where TConverter : new() // 限制 TConverter 必须有一个无参数的构造函数
{
    // 定义一个抽象属性 FalseValue，派生类必须实现此属性
    public abstract T FalseValue { get; set; }

    // 定义一个 BaseOnFalseValue 属性，获取或设置一个布尔值
    public bool BaseOnFalseValue
    {
        get { return (bool)this.GetValue(BaseOnFalseValueProperty); } // 获取 BaseOnFalseValue 属性的值
        set { this.SetValue(BaseOnFalseValueProperty, value); } // 设置 BaseOnFalseValue 属性的值
    }

    // 重写 Convert 方法，将值转换为目标类型
    protected override object Convert(object value, Type targetType, object parameter, CultureInfo culture)
    {
        if (!this.BaseOnFalseValue) // 如果 BaseOnFalseValue 属性为 false
        {
            return base.Convert(value, targetType, parameter, culture); // 调用基类的 Convert 方法
        }

        var falseValue = this.FalseValue; // 获取 FalseValue 属性的值
        // 如果 value 不等于 falseValue，则返回 !Equals(value, falseValue) 的结果异或 IsInverted 的值
        return !Equals(value, falseValue) ^ this.IsInverted;
    }

    // 重写 ConvertBack 方法，将值转换回原始类型
    protected override object ConvertBack(object value, Type targetType, object parameter, CultureInfo culture)
    {
        // 如果 value 等于 true，则返回 TrueValue 属性的值，否则返回 FalseValue 属性的值，取决于 IsInverted
        return true.Equals(value) ^ this.IsInverted ? this.TrueValue! : this.FalseValue!;
    }

    // 定义一个静态字段 BaseOnFalseValueProperty，表示 BaseOnFalseValue 属性的依赖属性
    public static readonly Property BaseOnFalseValueProperty = PropertyHelper.Create<bool, ValueToBoolConverterBase<T, TConverter>>(nameof(BaseOnFalseValueProperty));
}
```