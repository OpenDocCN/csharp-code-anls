# `.\better-genshin-impact\Build\MicaSetup\Core\ServiceManager.cs`

```cs
# 引入 Microsoft.Extensions.DependencyInjection 命名空间，提供依赖注入功能
﻿using Microsoft.Extensions.DependencyInjection;

# 定义命名空间 MicaSetup.Services，组织相关服务类
namespace MicaSetup.Services;

# 禁用警告 CS8618，防止未初始化的非 null 引用类型警告
#pragma warning disable CS8618

# 定义 ServiceManager 类，用于管理服务
public class ServiceManager
{
    # 定义静态属性 Services，用于存储服务提供者
    public static ServiceProvider Services { get; set; }

    # 定义泛型方法 GetService，用于获取指定类型的服务实例
    public static T GetService<T>()
    {
        # 从 Services 中获取指定类型的服务实例，并强制转换为非 null
        return Services.GetService<T>()!;
    }
}
```