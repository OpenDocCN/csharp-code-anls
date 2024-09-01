# `.\better-genshin-impact\Build\MicaSetup\Design\Converters\EnumToObjectConverter.cs`

```cs
# 引入系统基本功能和文化信息处理相关的命名空间
﻿using System;
using System.Globalization;
using System.Windows.Data;

# 定义转换器的命名空间
namespace MicaSetup.Design.Converters;

# 指定值转换器的源类型和目标类型
[ValueConversion(typeof(object), typeof(object))]
# 定义枚举到对象转换器类，继承自 StringToObjectConverter 类
public class EnumToObjectConverter : StringToObjectConverter
{
    # 重写 Convert 方法以实现自定义转换逻辑
    protected override object Convert(object value, Type targetType, object parameter, CultureInfo culture)
    {
        # 如果传入的值为空，返回未设置的值
        if (value == null)
        {
            return UnsetValue;
        }

        # 获取传入值的类型
        var type = value.GetType();
        # 获取枚举值的名称
        var key = Enum.GetName(type, value);
        # 调用基类的 Convert 方法，传入枚举名称进行转换
        return base.Convert(key, targetType, parameter, culture);
    }
}
```