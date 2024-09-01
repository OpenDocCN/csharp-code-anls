# `.\better-genshin-impact\Build\MicaSetup\Design\Converters\InvertedBoolConverter.cs`

```cs
# 导入用于数据绑定的命名空间
﻿using System.Windows.Data;

# 定义命名空间 MicaSetup.Design.Converters
namespace MicaSetup.Design.Converters;

# 指定此类作为值转换器，将布尔类型转换为布尔类型，并且该转换器对布尔值进行反转
[ValueConversion(typeof(bool), typeof(bool))]
# 定义 InvertedBoolConverter 类，继承自 BoolNegationConverter
public class InvertedBoolConverter : BoolNegationConverter
{
}
```