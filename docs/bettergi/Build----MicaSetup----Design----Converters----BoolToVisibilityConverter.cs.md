# `.\better-genshin-impact\Build\MicaSetup\Design\Converters\BoolToVisibilityConverter.cs`

```cs
# 引入 WPF 命名空间，包含窗口和控件的定义
﻿using System.Windows;
# 引入数据绑定命名空间
using System.Windows.Data;

# 定义转换器的命名空间
namespace MicaSetup.Design.Converters;

# 指定值转换器的源类型为 bool，目标类型为 Visibility
[ValueConversion(typeof(bool), typeof(Visibility))]
# BoolToVisibilityConverter 类继承自 BoolToValueConverter<Visibility>
public class BoolToVisibilityConverter : BoolToValueConverter<Visibility>
{
    # BoolToVisibilityConverter 的构造函数
    public BoolToVisibilityConverter()
    {
        # 设置 TrueValue 为 Visibility.Visible
        this.TrueValue = Visibility.Visible;
        # 设置 FalseValue 为 Visibility.Collapsed
        this.FalseValue = Visibility.Collapsed;
    }
}
```