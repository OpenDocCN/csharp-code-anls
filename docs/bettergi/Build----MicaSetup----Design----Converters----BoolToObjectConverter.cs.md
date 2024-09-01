# `.\better-genshin-impact\Build\MicaSetup\Design\Converters\BoolToObjectConverter.cs`

```cs
# 引入用于数据绑定的命名空间
﻿using System.Windows.Data;

# 定义命名空间 MicaSetup.Design.Converters
namespace MicaSetup.Design.Converters;

# 指定此类用于将布尔值转换为对象的值转换器
[ValueConversion(typeof(bool), typeof(object))]
# 定义一个布尔到对象的转换器类，继承自 BoolToValueConverter，泛型参数为对象
public class BoolToObjectConverter : BoolToValueConverter<object>
{
}
```