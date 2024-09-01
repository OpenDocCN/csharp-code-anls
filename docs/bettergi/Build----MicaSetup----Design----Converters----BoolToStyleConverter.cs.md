# `.\better-genshin-impact\Build\MicaSetup\Design\Converters\BoolToStyleConverter.cs`

```cs
# 使用 C# 的 WPF 命名空间和数据绑定命名空间
﻿using System.Windows;
using System.Windows.Data;

# 定义命名空间 MicaSetup.Design.Converters
namespace MicaSetup.Design.Converters;

# 指定值转换器的输入类型为 bool，输出类型为 Style
[ValueConversion(typeof(bool), typeof(Style))]
# 定义 BoolToStyleConverter 类，继承自 BoolToValueConverter<Style>
public class BoolToStyleConverter : BoolToValueConverter<Style>
{
}
```