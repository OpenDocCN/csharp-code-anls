# `.\better-genshin-impact\Build\MicaSetup\Design\Converters\Abstraction\SingletonConverterBase.cs`

```cs
# 使用 System 命名空间下的类
﻿using System;
# 使用 System.Threading 命名空间下的类
using System.Threading;

# 定义 MicaSetup.Design.Converters 命名空间
namespace MicaSetup.Design.Converters;

# 定义一个抽象的泛型类 SingletonConverterBase，继承自 ConverterBase
# TConverter 是一个泛型参数，必须有一个无参数的构造函数
public abstract class SingletonConverterBase<TConverter> : ConverterBase
    where TConverter : new()
{
    # 定义一个静态的只读 Lazy<TConverter> 实例，使用延迟初始化
    # Lazy<TConverter> 用于实现线程安全的延迟实例化
    private static readonly Lazy<TConverter> InstanceConstructor = new(() =>
    {
        # 实例化 TConverter 类型的对象
        return new TConverter();
    }, LazyThreadSafetyMode.PublicationOnly);

    # 定义一个公共的静态属性 Instance，返回实例化的 TConverter 对象
    public static TConverter Instance => InstanceConstructor.Value;
}
```