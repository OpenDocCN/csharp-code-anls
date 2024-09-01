# `.\better-genshin-impact\BetterGenshinImpact\Model\Singleton.cs`

```cs
# 使用 C# 的命名空间和类定义
﻿using System;
using System.Threading;

namespace BetterGenshinImpact.Model;

/// <summary>
/// 由于 C# 的 DI 过于难用，bgi代码中依旧存在使用大量原始单例的对象
/// 给他们实现一个通用的单例模式
/// </summary>
/// <typeparam name="T"></typeparam>
public class Singleton<T> where T : class
{
    # 保护的静态实例变量，可能为空
    protected static T? _instance;
    # 保护的静态同步对象，可能为空
    protected static object? syncRoot;

    # 提供对单例实例的访问，使用 LazyInitializer 确保线程安全初始化
    public static T Instance => LazyInitializer.EnsureInitialized(ref _instance, ref syncRoot, CreateInstance);

    # 创建实例的静态方法
    protected static T CreateInstance()
    {
        # 通过反射创建类型 T 的实例，忽略私有构造函数
        return (T)Activator.CreateInstance(typeof(T), true)!;
    }

    # 销毁单例实例，设置实例为 null
    public static void DestroyInstance()
    {
        _instance = null;
    }
}
```