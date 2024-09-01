# `.\better-genshin-impact\Build\MicaSetup\Design\Converters\BoolToFontWeightConverter.cs`

```cs
# 使用 C# 的 Windows Presentation Foundation (WPF) 命名空间
﻿using System.Windows;
using System.Windows.Data;

# 定义一个命名空间 MicaSetup.Design.Converters
namespace MicaSetup.Design.Converters;

# 指定一个值转换器，将布尔值转换为字体粗细
[ValueConversion(typeof(bool), typeof(FontWeight))]
public class BoolToFontWeightConverter : BoolToValueConverter<FontWeight>
{
    # 构造函数，初始化布尔值到字体粗细的转换
    public BoolToFontWeightConverter()
    {
        # 布尔值为 true 时字体粗细设置为粗体
        this.TrueValue = FontWeights.Bold;
        # 布尔值为 false 时字体粗细设置为正常
        this.FalseValue = FontWeights.Normal;
    }
}
```