# `.\better-genshin-impact\Build\MicaSetup\Design\Converters\FirstOrDefaultConverter.cs`

```cs
# 引入必要的命名空间
﻿using System;
using System.Collections;
using System.Globalization;
using System.Windows.Data;

# 定义命名空间
namespace MicaSetup.Design.Converters;

# 声明一个值转换器，将 IEnumerable 类型转换为 object 类型
[ValueConversion(typeof(IEnumerable), typeof(object))]
# 定义 FirstOrDefaultConverter 类，继承自 SingletonConverterBase
public class FirstOrDefaultConverter : SingletonConverterBase<FirstOrDefaultConverter>
{
    # 重写 Convert 方法实现具体的转换逻辑
    protected override object Convert(object value, Type targetType, object parameter, CultureInfo culture)
    {
        # 判断传入的值是否为 IEnumerable 类型
        if (value is IEnumerable enumerable)
        {
            # 获取 IEnumerator 实例用于遍历 IEnumerable
            var enumerator = enumerable.GetEnumerator();
            {
                # 尝试移动到集合的下一项
                if (enumerator.MoveNext())
                {
                    # 返回当前项（集合的第一个元素）
                    return enumerator.Current;
                }
            }
        }

        # 如果集合为空或无法转换，返回未设置值
        return UnsetValue;
    }
}
```