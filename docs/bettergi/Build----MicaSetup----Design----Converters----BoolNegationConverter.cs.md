# `.\better-genshin-impact\Build\MicaSetup\Design\Converters\BoolNegationConverter.cs`

```cs
# 引入数据绑定相关的命名空间
﻿using System.Windows.Data;

# 定义命名空间 MicaSetup.Design.Converters
namespace MicaSetup.Design.Converters;

# 指定该类是一个值转换器，将 bool 类型转换为 bool 类型
[ValueConversion(typeof(bool), typeof(bool))]
# BoolNegationConverter 类继承自 BoolToValueConverter 泛型类，泛型参数为 bool 类型
public class BoolNegationConverter : BoolToValueConverter<bool>
{
    # BoolNegationConverter 构造函数
    public BoolNegationConverter()
    {
        # 设置 TrueValue 属性为 true
        this.TrueValue = true;
        # 设置 FalseValue 属性为 false
        this.FalseValue = false;
        # 设置 IsInverted 属性为 true，表示布尔值取反
        this.IsInverted = true;
    }
}
```