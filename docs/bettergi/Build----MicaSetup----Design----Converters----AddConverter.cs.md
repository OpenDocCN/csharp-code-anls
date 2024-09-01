# `.\better-genshin-impact\Build\MicaSetup\Design\Converters\AddConverter.cs`

```cs
﻿using System;  // 引入 System 命名空间，提供基本的类和功能
using System.Globalization;  // 引入 System.Globalization 命名空间，提供文化相关功能
using System.Windows.Data;  // 引入 System.Windows.Data 命名空间，提供数据绑定功能

namespace MicaSetup.Design.Converters;  // 定义命名空间，用于组织代码

[ValueConversion(typeof(double), typeof(double))]  // 指定该转换器将 double 类型转换为 double 类型
public class AddConverter : SingletonConverterBase<AddConverter>  // 定义 AddConverter 类，继承自 SingletonConverterBase，支持单例模式
{
    protected override object Convert(object value, Type targetType, object parameter, CultureInfo culture)  // 重写 Convert 方法，执行实际的转换操作
    {
        if (double.TryParse(value?.ToString(), NumberStyles.Any, culture, out var basis)  // 尝试将 value 转换为 double 类型，成功则存储在 basis 中
            && double.TryParse(parameter?.ToString(), NumberStyles.Any, culture, out var subtract))  // 尝试将 parameter 转换为 double 类型，成功则存储在 subtract 中
        {
            return basis + subtract;  // 如果转换成功，返回两者的和
        }

        return UnsetValue;  // 如果转换失败，返回未设置的值
    }
}
```