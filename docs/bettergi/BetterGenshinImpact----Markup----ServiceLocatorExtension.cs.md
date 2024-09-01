# `.\better-genshin-impact\BetterGenshinImpact\Markup\ServiceLocatorExtension.cs`

```cs
# 使用 C# 和 XAML 进行标记扩展，提供服务定位器功能
﻿using System;
using System.Windows.Markup;

namespace BetterGenshinImpact.Markup;

# 指定这个 MarkupExtension 扩展返回的类型是 object
[MarkupExtensionReturnType(typeof(object))]
public class ServiceLocatorExtension : MarkupExtension
{
    # 用于指定要获取服务的类型
    public Type Type { get; set; } = null!;

    # 默认构造函数
    public ServiceLocatorExtension()
    {
    }

    # 实现 MarkupExtension 的 ProvideValue 方法
    public override object ProvideValue(IServiceProvider serviceProvider)
    {
        # 抛出异常如果 Type 属性为 null
        ArgumentNullException.ThrowIfNull(Type);
        # 调用应用程序的 GetService 方法获取指定类型的服务，并返回
        return App.GetService(Type)!;
    }
}
```