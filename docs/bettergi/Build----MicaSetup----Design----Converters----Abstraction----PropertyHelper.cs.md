# `.\better-genshin-impact\Build\MicaSetup\Design\Converters\Abstraction\PropertyHelper.cs`

```cs
# 引入 WPF 框架的基本命名空间
﻿using System.Windows;
# 引入 WPF 的 DependencyProperty 类型
using Property = System.Windows.DependencyProperty;

# 定义命名空间 MicaSetup.Design.Converters
namespace MicaSetup.Design.Converters;

# 禁用 WPF0011 和 WPF0015 警告
#pragma warning disable WPF0011
#pragma warning disable WPF0015

# 定义一个内部静态类 PropertyHelper
internal static class PropertyHelper
{
    # 定义一个静态方法，用于创建 DependencyProperty 实例
    public static Property Create<T, TParent>(string name, T defaultValue) =>
        # 注册一个新的 DependencyProperty，使用指定的名称、类型、拥有者类型和默认值
        Property.Register(name, typeof(T), typeof(TParent), new PropertyMetadata(defaultValue));

    # 定义一个重载的静态方法，用于创建 DependencyProperty 实例
    public static Property Create<T, TParent>(string name) => Create<T, TParent>(name, default!);
}
```