# `.\better-genshin-impact\Build\MicaSetup\Design\Converters\BoolToValueConverter.cs`

```cs
# 引入 WPF 框架中的命名空间，用于窗口和数据绑定
﻿using System.Windows;
using System.Windows.Data;

# 定义命名空间
namespace MicaSetup.Design.Converters;

# 指定值转换器的源类型和目标类型
[ValueConversion(typeof(bool), typeof(object))]
# 定义泛型布尔值到值的转换器类，继承自 BoolToValueConverterBase
public class BoolToValueConverter<T> : BoolToValueConverterBase<T, BoolToValueConverter<T>>
{
    # 定义 TrueValueProperty 静态字段，注册一个依赖属性用于表示布尔值为 true 时的转换值
    public static readonly DependencyProperty TrueValueProperty = DependencyProperty.Register(
        nameof(TrueValue), # 属性名称
        typeof(T), # 属性类型
        typeof(BoolToValueConverter<T>), # 所属类型
        new PropertyMetadata(default(T))); # 默认值

    # 定义 FalseValueProperty 静态字段，注册一个依赖属性用于表示布尔值为 false 时的转换值
    public static readonly DependencyProperty FalseValueProperty = DependencyProperty.Register(
        nameof(FalseValue), # 属性名称
        typeof(T), # 属性类型
        typeof(BoolToValueConverter<T>), # 所属类型
        new PropertyMetadata(default(T))); # 默认值

    # 定义 IsInvertedProperty 静态字段，注册一个依赖属性用于表示是否反转布尔值
    public static readonly DependencyProperty IsInvertedProperty = DependencyProperty.Register(
        nameof(IsInverted), # 属性名称
        typeof(bool), # 属性类型
        typeof(BoolToValueConverter<T>), # 所属类型
        new PropertyMetadata(false)); # 默认值

    # 重写 TrueValue 属性的 getter 和 setter，操作 TrueValueProperty 依赖属性
    public override T TrueValue
    {
        get => (T)this.GetValue(TrueValueProperty); # 获取 TrueValueProperty 的值
        set => this.SetValue(TrueValueProperty, value); # 设置 TrueValueProperty 的值
    }

    # 重写 FalseValue 属性的 getter 和 setter，操作 FalseValueProperty 依赖属性
    public override T FalseValue
    {
        get => (T)this.GetValue(FalseValueProperty); # 获取 FalseValueProperty 的值
        set => this.SetValue(FalseValueProperty, value); # 设置 FalseValueProperty 的值
    }

    # 重写 IsInverted 属性的 getter 和 setter，操作 IsInvertedProperty 依赖属性
    public override bool IsInverted
    {
        get => (bool)this.GetValue(IsInvertedProperty); # 获取 IsInvertedProperty 的值
        set => this.SetValue(IsInvertedProperty, value); # 设置 IsInvertedProperty 的值
    }
}
```