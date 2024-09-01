# `.\better-genshin-impact\Build\MicaSetup\Design\Converters\BoolToThicknessConverter.cs`

```cs
# 引入 WPF 框架的命名空间，用于窗口和控件的定义
﻿using System.Windows;
# 引入 WPF 数据绑定的命名空间，用于数据转换
using System.Windows.Data;

# 定义命名空间 MicaSetup.Design.Converters
namespace MicaSetup.Design.Converters;

# 定义一个值转换器，将 bool 类型转换为 Thickness 类型
[ValueConversion(typeof(bool), typeof(Thickness))]
# 定义 BoolToThicknessConverter 类，继承自 BoolToValueConverter 泛型类，转换类型为 Thickness
public class BoolToThicknessConverter : BoolToValueConverter<Thickness>
{
}
```