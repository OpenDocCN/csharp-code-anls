# `.\better-genshin-impact\Build\MicaSetup\Design\Converters\IntegerToBoolConverter.cs`

```cs
# 引入 WPF 数据绑定命名空间
﻿using System.Windows.Data;

# 定义一个命名空间，用于组织相关的类
namespace MicaSetup.Design.Converters;

# 指定一个值转换器属性，表示这个类用于将整数类型转换为布尔类型
[ValueConversion(typeof(int), typeof(bool))]
# 定义一个将整数转换为布尔值的转换器类
public class IntegerToBoolConverter : ValueToBoolConverter<int>
{
}
```