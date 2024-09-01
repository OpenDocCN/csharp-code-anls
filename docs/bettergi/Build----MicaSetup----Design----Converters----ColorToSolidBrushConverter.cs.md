# `.\better-genshin-impact\Build\MicaSetup\Design\Converters\ColorToSolidBrushConverter.cs`

```cs
# 引入必要的命名空间
﻿using System;
using System.Globalization;
using System.Windows.Data;
using System.Windows.Media;

namespace MicaSetup.Design.Converters;

# 指定值转换的类型，将字符串转换为 SolidColorBrush
[ValueConversion(typeof(string), typeof(SolidColorBrush))]
# 定义一个将 Color 类型转换为 SolidColorBrush 的转换器
public sealed class ColorToSolidBrushConverter : ConverterBase
{
    # 表示是否冻结 SolidColorBrush 对象的属性
    public bool Freeze { get; set; } = true;

    # 实现将源对象值转换为目标类型的方法
    protected override object Convert(object value, Type targetType, object parameter, CultureInfo culture)
    {
        # 检查传入的值是否为 Color 类型
        if (value is Color color)
        {
            # 使用 Color 创建 SolidColorBrush 对象
            SolidColorBrush brush = new(color);

            # 如果 Freeze 属性为 true，则冻结 SolidColorBrush 对象
            if (Freeze)
            {
                brush.Freeze();
            }
            # 返回创建的 SolidColorBrush 对象
            return brush;
        }
        # 如果传入的值不是 Color 类型，则直接返回传入的值
        return value;
    }

    # 实现将源对象值转换回原始类型的方法
    protected override object ConvertBack(object value, Type targetType, object parameter, CultureInfo culture)
    {
        # 检查传入的值是否为 SolidColorBrush 类型
        if (value is SolidColorBrush brush)
        {
            # 返回 SolidColorBrush 对象的 Color 属性
            return brush.Color;
        }
        # 如果传入的值不是 SolidColorBrush 类型，则返回默认的 Color 值
        return default(Color);
    }
}
```