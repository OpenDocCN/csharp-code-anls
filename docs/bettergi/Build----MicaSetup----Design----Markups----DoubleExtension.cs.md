# `.\better-genshin-impact\Build\MicaSetup\Design\Markups\DoubleExtension.cs`

```cs
# 引入系统命名空间，提供基础功能
﻿using System;
# 引入 XAML 标记扩展相关功能
using System.Windows.Markup;

# 定义一个命名空间，组织代码
namespace MicaSetup.Design.Markups;

# 指定此标记扩展返回类型为 double
[MarkupExtensionReturnType(typeof(double))]
# 定义一个密封类 DoubleExtension，继承自 MarkupExtension
public sealed class DoubleExtension : MarkupExtension
{
    # 定义一个双精度浮点数属性 Value
    public double Value { get; set; }

    # 重写 MarkupExtension 类中的 ProvideValue 方法
    public override object ProvideValue(IServiceProvider serviceProvider)
    {
        # 返回属性 Value 的值
        return Value;
    }
}
```