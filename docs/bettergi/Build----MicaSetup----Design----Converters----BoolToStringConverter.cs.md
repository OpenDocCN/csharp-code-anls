# `.\better-genshin-impact\Build\MicaSetup\Design\Converters\BoolToStringConverter.cs`

```cs
# 引入 WPF 数据绑定命名空间
﻿using System.Windows.Data;

# 定义 MicaSetup.Design.Converters 命名空间
namespace MicaSetup.Design.Converters;

# 标记该类作为值转换器，指定输入类型为 bool，输出类型为 string
[ValueConversion(typeof(bool), typeof(string))]
# 定义 BoolToStringConverter 类，继承自 BoolToValueConverter<string>
public class BoolToStringConverter : BoolToValueConverter<string>
{
    # 该类继承了 BoolToValueConverter<string> 的功能，未做任何修改
}
```