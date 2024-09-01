# `.\better-genshin-impact\Build\MicaSetup\Natives\Shell\Dialogs\PropertySystem\SystemProperties.cs`

```cs
# 引入 System 命名空间，提供基本功能和数据类型
﻿using System;
# 引入 System.Runtime.InteropServices 命名空间，提供与非托管代码交互的功能
using System.Runtime.InteropServices;

# 定义一个命名空间 MicaSetup.Shell.Dialogs
namespace MicaSetup.Shell.Dialogs;

# 定义一个静态类 SystemProperties
public static class SystemProperties
{
    # 定义一个静态方法 GetPropertyDescription，根据 PropertyKey 获取属性描述
    public static ShellPropertyDescription GetPropertyDescription(PropertyKey propertyKey) => ShellPropertyDescriptionsCache.Cache.GetPropertyDescription(propertyKey);

    # 定义一个静态方法 GetPropertyDescription，根据规范名称获取属性描述
    public static ShellPropertyDescription GetPropertyDescription(string canonicalName)
    {
        # 调用外部方法 PSGetPropertyKeyFromName，将规范名称转换为 PropertyKey，并存储结果
        var result = PropertySystemNativeMethods.PSGetPropertyKeyFromName(canonicalName, out var propKey);

        # 如果转换失败，抛出异常
        if (!CoreErrorHelper.Succeeded(result))
        {
            throw new ArgumentException(LocalizedMessages.ShellInvalidCanonicalName, Marshal.GetExceptionForHR(result));
        }
        # 根据 PropertyKey 获取属性描述
        return ShellPropertyDescriptionsCache.Cache.GetPropertyDescription(propKey);
    }
}
```