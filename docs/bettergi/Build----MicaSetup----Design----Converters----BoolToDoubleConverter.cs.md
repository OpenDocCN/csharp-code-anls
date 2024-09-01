# `.\better-genshin-impact\Build\MicaSetup\Design\Converters\BoolToDoubleConverter.cs`

```cs
# 引入用于数据绑定转换的命名空间
﻿using System.Windows.Data;

# 定义命名空间，包含与 MicaSetup 设计相关的转换器
namespace MicaSetup.Design.Converters;

# 定义一个值转换器，指定输入值类型为 bool，输出值类型为 double
[ValueConversion(typeof(bool), typeof(double))]
# BoolToDoubleConverter 类继承自 BoolToValueConverter<double>，用于将 bool 类型转换为 double 类型
public class BoolToDoubleConverter : BoolToValueConverter<double>
{
}
```