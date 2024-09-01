# `.\better-genshin-impact\Build\MicaSetup\Design\Converters\DebugConverter.cs`

```cs
# 引入系统命名空间
﻿using System;
# 引入调试命名空间，用于调试信息输出
using System.Diagnostics;
# 引入数据绑定命名空间
using System.Windows.Data;

# 定义命名空间
namespace MicaSetup.Design.Converters;

# 声明一个值转换器，指定输入输出类型为 object
[ValueConversion(typeof(object), typeof(object))]
# 定义一个 DebugConverter 类，继承自 SingletonConverterBase<DebugConverter>
public class DebugConverter : SingletonConverterBase<DebugConverter>
{
    # 重写 Convert 方法，用于将源对象转换为目标对象
    protected override object Convert(object value, Type targetType, object parameter, System.Globalization.CultureInfo culture)
    {
        # 输出调试信息，包括源对象、目标类型、参数和文化信息
        Debug.WriteLine("DebugConverter.Convert(value={0}, targetType={1}, parameter={2}, culture={3}",
            value ?? "null",
            (object)targetType ?? "null",
            parameter ?? "null",
            (object)culture ?? "null");

        # 返回源对象
        return value!;
    }

    # 重写 ConvertBack 方法，用于将目标对象转换回源对象
    protected override object ConvertBack(object value, Type targetType, object parameter, System.Globalization.CultureInfo culture)
    {
        # 输出调试信息，包括目标对象、目标类型、参数和文化信息
        Debug.WriteLine("DebugConverter.ConvertBack(value={0}, targetType={1}, parameter={2}, culture={3}",
             value ?? "null",
             (object)targetType ?? "null",
             parameter ?? "null",
             (object)culture ?? "null");

        # 返回目标对象
        return value!;
    }
}
```