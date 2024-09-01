# `.\better-genshin-impact\Build\MicaSetup\Design\Converters\VisibilityInverter.cs`

```cs
# 使用 WPF 的命名空间和数据绑定
﻿using System.Windows;
using System.Windows.Data;

# 定义命名空间
namespace MicaSetup.Design.Converters;

# 指定值转换器的输入类型和输出类型
[ValueConversion(typeof(Visibility), typeof(string))]
# 创建一个继承自 BoolToValueConverter 的类，用于反转 Visibility 值
public class VisibilityInverter : BoolToValueConverter<Visibility>
{
    # 构造函数初始化反转器
    public VisibilityInverter()
    {
        # 设置 TrueValue 为 Visibility.Visible
        this.TrueValue = Visibility.Visible;
        # 设置 FalseValue 为 Visibility.Collapsed
        this.FalseValue = Visibility.Collapsed;
        # 设定反转标志为 true，表示反转操作
        this.IsInverted = true;
    }
}
```