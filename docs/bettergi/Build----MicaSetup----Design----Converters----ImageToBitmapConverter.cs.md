# `.\better-genshin-impact\Build\MicaSetup\Design\Converters\ImageToBitmapConverter.cs`

```cs
# 引入必要的命名空间
﻿using System;
using System.Drawing;
using System.Globalization;
using System.Windows.Data;

# 定义命名空间 MicaSetup.Design.Converters
namespace MicaSetup.Design.Converters;

# 指定转换器将类型 Image 转换为 Bitmap
[ValueConversion(typeof(Image), typeof(Bitmap))]
# 定义一个封闭的类，用于将 Image 对象转换为 Bitmap 对象，继承自 SingletonConverterBase
public sealed class ImageToBitmapConverter : SingletonConverterBase<ImageToBitmapConverter>
{
    # 实现基类的 Convert 方法，将对象转换为目标类型
    protected override object Convert(object value, Type targetType, object parameter, CultureInfo culture)
    {
        # 如果输入值是 Image 类型，则创建一个 Bitmap 对象并返回
        if (value is Image image)
        {
            return new Bitmap(image);
        }
        # 如果输入值已经是 Bitmap 类型，则直接返回
        else if (value is Bitmap)
        {
            return value;
        }
        # 如果输入值既不是 Image 也不是 Bitmap，则返回 null
        return null!;
    }
}
```