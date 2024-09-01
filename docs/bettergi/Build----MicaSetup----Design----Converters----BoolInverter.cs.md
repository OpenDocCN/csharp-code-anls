# `.\better-genshin-impact\Build\MicaSetup\Design\Converters\BoolInverter.cs`

```cs
# 引入数据绑定的命名空间
﻿using System.Windows.Data;

# 定义命名空间 MicaSetup.Design.Converters
namespace MicaSetup.Design.Converters;

# 标记此类为值转换器，指定输入和输出类型都是布尔值
[ValueConversion(typeof(bool), typeof(bool))]
# 定义 BoolInverter 类，继承自 BoolNegationConverter
public class BoolInverter : BoolNegationConverter
{
}
```