# `.\better-genshin-impact\Build\MicaSetup\Design\Converters\SubtractConverter.cs`

```cs
# 引入必要的命名空间
﻿using System;
using System.Globalization;
using System.Windows.Data;

namespace MicaSetup.Design.Converters;

# 指定值转换器的输入和输出类型
[ValueConversion(typeof(object), typeof(object))]
# 定义 SubtractConverter 类，继承自 SingletonConverterBase
public class SubtractConverter : SingletonConverterBase<SubtractConverter>
{
    # 重写基类的 Convert 方法，实现具体的转换逻辑
    protected override object Convert(object value, Type targetType, object parameter, CultureInfo culture)
    {
        # 尝试将输入值转换为 double 类型，如果成功，赋值给 basis
        if (double.TryParse(value?.ToString(), NumberStyles.Any, culture, out var basis)
            # 尝试将参数值转换为 double 类型，如果成功，赋值给 subtract
            && double.TryParse(parameter?.ToString(), NumberStyles.Any, culture, out var subtract))
        {
            # 返回 basis 减去 subtract 的结果
            return basis - subtract;
        }

        # 如果转换失败，则返回 UnsetValue 表示未设置
        return UnsetValue;
    }
}
```