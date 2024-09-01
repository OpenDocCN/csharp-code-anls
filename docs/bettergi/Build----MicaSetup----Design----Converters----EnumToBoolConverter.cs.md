# `.\better-genshin-impact\Build\MicaSetup\Design\Converters\EnumToBoolConverter.cs`

```cs
# 引入必要的命名空间
﻿using System;
using System.Globalization;
using System.Windows.Data;

# 定义命名空间
namespace MicaSetup.Design.Converters;

# 指定值转换器的输入类型为 object，输出类型为 bool
[ValueConversion(typeof(object), typeof(bool))]
# 定义枚举到布尔值转换器类，继承自 SingletonConverterBase
public class EnumToBoolConverter : SingletonConverterBase<EnumToBoolConverter>
{
    # 重写 Convert 方法以进行值转换
    protected override object Convert(object value, Type targetType, object parameter, CultureInfo culture)
    {
        # 如果参数是字符串类型
        if (parameter is string parameterString)
        {
            # 检查枚举值是否在枚举定义中
            if (Enum.IsDefined(value.GetType(), value) == false)
            {
                # 如果枚举值不在定义中，返回未设置值
                return UnsetValue;
            }

            # 解析参数字符串为枚举值
            var parameterValue = Enum.Parse(value.GetType(), parameterString);

            # 返回参数值是否等于输入值的比较结果
            return parameterValue.Equals(value);
        }

        # 如果参数不是字符串，返回未设置值
        return UnsetValue;
    }

    # 重写 ConvertBack 方法以进行值转换
    protected override object ConvertBack(object value, Type targetType, object parameter, CultureInfo culture)
    {
        # 如果参数是字符串类型
        if (parameter is string parameterString)
        {
            # 解析参数字符串为目标类型的枚举值
            return Enum.Parse(targetType, parameterString);
        }

        # 如果参数不是字符串，返回未设置值
        return UnsetValue;
    }
}
```