# `.\better-genshin-impact\Build\MicaSetup\Design\Converters\DoubleToBoolConverter.cs`

```cs
# 引入 System.Windows.Data 命名空间，包含数据绑定相关的功能
﻿using System.Windows.Data;

# 定义一个名为 MicaSetup.Design.Converters 的命名空间
namespace MicaSetup.Design.Converters;

# 声明一个值转换器类，将 double 类型转换为 bool 类型
# 这个转换器用于数据绑定，将 double 类型的值转换为 bool 类型的值
[ValueConversion(typeof(double), typeof(bool))]
public class DoubleToBoolConverter : ValueToBoolConverter<double>
{
}
```