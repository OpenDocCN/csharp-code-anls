# `.\better-genshin-impact\Build\MicaSetup\Natives\Shell\Dialogs\PropertySystem\ShellPropertyDescriptionsCache.cs`

```cs
# 引入需要的命名空间
﻿using System.Collections.Generic;

# 定义命名空间
namespace MicaSetup.Shell.Dialogs;

# 禁用 CS8618 警告，这个警告通常提示可能的空引用问题
#pragma warning disable CS8618

# 定义内部类 ShellPropertyDescriptionsCache
internal class ShellPropertyDescriptionsCache
{
    # 静态字段，用于存储单例实例
    private static ShellPropertyDescriptionsCache cacheInstance;

    # 只读字段，用于存储属性键到属性描述的字典
    private readonly IDictionary<PropertyKey, ShellPropertyDescription> propsDictionary;

    # 私有构造函数，初始化字典
    private ShellPropertyDescriptionsCache() => propsDictionary = new Dictionary<PropertyKey, ShellPropertyDescription>();

    # 公共静态属性，用于获取单例实例
    public static ShellPropertyDescriptionsCache Cache
    {
        get
        {
            # 如果单例实例为空，则创建一个新的实例
            if (cacheInstance == null)
            {
                cacheInstance = new ShellPropertyDescriptionsCache();
            }
            # 返回单例实例
            return cacheInstance;
        }
    }

    # 公共方法，根据属性键获取属性描述
    public ShellPropertyDescription GetPropertyDescription(PropertyKey key)
    {
        # 如果字典中不存在该键，则添加新的属性描述
        if (!propsDictionary.ContainsKey(key))
        {
            propsDictionary.Add(key, new ShellPropertyDescription(key));
        }
        # 返回对应键的属性描述
        return propsDictionary[key];
    }
}
```