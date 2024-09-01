# `.\better-genshin-impact\Build\MicaSetup\Design\Converters\MathRoundConverter.cs`

```cs
# 使用 .NET 内部类实现 IValueConverter 接口
internal sealed class MathRoundConverter : IValueConverter
{
    # 实现 IValueConverter 接口中的 Convert 方法
    public object Convert(object value, Type targetType, object parameter, CultureInfo culture)
    {
        # 定义一个可空的 double 类型变量
        double? valueNullable = default;

        # 判断传入值的类型并赋值给可空 double 变量
        if (value is float @float)
        {
            valueNullable = @float;
        }
        else if (value is double @double)
        {
            valueNullable = @double;
        }
        else if (value is int @int)
        {
            valueNullable = @int;
        }

        # 如果可空 double 变量不为 null
        if (valueNullable != null)
        {
            # 定义一个整数变量用于存储小数位数
            int dec = default;

            # 根据参数决定小数位数
            if (parameter is null)
            {
                dec = default;
            }
            else if (parameter is int decFromInt)
            {
                dec = decFromInt;
            }
            else if (parameter is string decString)
            {
                # 尝试将参数字符串转换为整数
                if (int.TryParse(decString, out int decFromString))
                {
                    dec = decFromString;
                }
            }
            # 使用指定的小数位数四舍五入并转换为字符串
            return Math.Round(valueNullable.Value, dec).ToString();
        }
        # 如果可空 double 变量为 null，返回原始值的字符串表示
        return value?.ToString()!;
    }

    # 实现 IValueConverter 接口中的 ConvertBack 方法
    public object ConvertBack(object value, Type targetType, object parameter, CultureInfo culture)
    {
        # 不实现 ConvertBack 方法，抛出未实现异常
        throw new NotImplementedException();
    }
}
```