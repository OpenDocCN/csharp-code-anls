# `.\better-genshin-impact\Build\MicaSetup\Design\Converters\StringToDecimalConverter.cs`

```cs
# 导入系统相关的命名空间
﻿using System;
using System.Globalization;
using System.Windows.Data;

# 命名空间定义
namespace MicaSetup.Design.Converters;

# 标记该转换器用于将字符串转换为十进制类型
[ValueConversion(typeof(string), typeof(decimal))]
# 定义字符串到十进制转换器类，继承自 SingletonConverterBase
public class StringToDecimalConverter : SingletonConverterBase<StringToDecimalConverter>
{
    # 定义默认的数字样式
    private static readonly NumberStyles DefaultNumberStyles = NumberStyles.Any;

    # 重写 Convert 方法，将输入值转换为目标类型
    protected override object Convert(object value, Type targetType, object parameter, CultureInfo culture)
    {
        # 将输入值尝试转换为可空的十进制数
        var dec = value as decimal?;
        if (dec != null)
        {
            # 如果输入值是十进制数，返回其字符串表示形式
            return dec.Value.ToString("G", culture ?? CultureInfo.InvariantCulture);
        }

        # 如果输入值是字符串类型
        if (value is string str)
        {
            # 尝试将字符串解析为十进制数
            if (decimal.TryParse(str, DefaultNumberStyles, culture ?? CultureInfo.InvariantCulture, out var result))
            {
                return result;
            }

            # 如果解析失败，返回默认的十进制值
            return result;
        }

        # 如果输入值不是预期的类型，返回未设置的值
        return UnsetValue;
    }

    # 重写 ConvertBack 方法，将值转换回源类型
    protected override object ConvertBack(object value, Type targetType, object parameter, CultureInfo culture)
    {
        # 直接调用 Convert 方法进行转换
        return this.Convert(value, targetType, parameter, culture);
    }
}
```