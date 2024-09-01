# `.\better-genshin-impact\Build\MicaSetup\Design\Converters\BoolToBrushConverter.cs`

```cs
# 引用用于数据绑定的库
﻿using System.Windows.Data;
# 引用用于绘图和颜色的库
using System.Windows.Media;

# 定义命名空间 MicaSetup.Design.Converters
namespace MicaSetup.Design.Converters;

# 定义一个值转换器，将布尔值转换为画刷类型
[ValueConversion(typeof(bool), typeof(Brush))]
# 定义 BoolToBrushConverter 类，继承自 BoolToValueConverter 泛型类，泛型参数为 Brush 类型
public class BoolToBrushConverter : BoolToValueConverter<Brush>
{
}
```