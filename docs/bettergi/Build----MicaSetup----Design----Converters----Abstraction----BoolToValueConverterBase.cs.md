# `.\better-genshin-impact\Build\MicaSetup\Design\Converters\Abstraction\BoolToValueConverterBase.cs`

```cs
# 定义一个抽象类 BoolToValueConverterBase，继承自 SingletonConverterBase
public abstract class BoolToValueConverterBase<T, TConverter> : SingletonConverterBase<TConverter> where TConverter : new()
{
    # 定义一个抽象属性 TrueValue，表示布尔值为 true 时的目标值
    public abstract T TrueValue { get; set; }

    # 定义一个抽象属性 FalseValue，表示布尔值为 false 时的目标值
    public abstract T FalseValue { get; set; }

    # 定义一个抽象属性 IsInverted，表示是否反转转换逻辑
    public abstract bool IsInverted { get; set; }

    # 重写 Convert 方法，将布尔值转换为指定类型的值
    protected override object Convert(object value, Type targetType, object parameter, CultureInfo culture)
    {
        # 默认为 FalseValue
        var returnValue = this.FalseValue;

        # 检查传入的值是否为布尔值
        if (value is bool boolValue)
        {
            # 根据 IsInverted 属性决定转换逻辑
            if (this.IsInverted)
            {
                returnValue = boolValue ? this.FalseValue : this.TrueValue;
            }
            else
            {
                returnValue = boolValue ? this.TrueValue : this.FalseValue;
            }
        }

        # 返回转换后的值
        return returnValue!;
    }

    # 重写 ConvertBack 方法，将目标值转换回布尔值
    protected override object ConvertBack(object value, Type targetType, object parameter, CultureInfo culture)
    {
        # 默认为 false
        bool returnValue = false;

        # 检查传入的值是否为非 null
        if (value != null)
        {
            # 根据 IsInverted 属性决定转换逻辑
            if (this.IsInverted)
            {
                returnValue = value.Equals(this.FalseValue);
            }
            else
            {
                returnValue = value.Equals(this.TrueValue);
            }
        }

        # 返回转换后的布尔值
        return returnValue;
    }
}
```