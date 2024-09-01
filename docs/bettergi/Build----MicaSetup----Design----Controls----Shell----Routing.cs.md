# `.\better-genshin-impact\Build\MicaSetup\Design\Controls\Shell\Routing.cs`

```cs
﻿using Microsoft.Extensions.DependencyInjection; // 引入依赖注入相关的扩展方法
using System; // 引入基本系统功能
using System.Collections.Generic; // 引入集合类
using System.Linq; // 引入 LINQ 查询功能
using System.Windows; // 引入 WPF 窗体功能

namespace MicaSetup.Design.Controls; // 定义命名空间

public static class Routing // 定义静态类 Routing
{
    public static ServiceProvider Provider { get; internal set; } = null!; // 定义内部可设置的静态属性 Provider，用于存储服务提供者
    public static WeakReference<ShellControl> Shell { get; internal set; } = null!; // 定义内部可设置的静态属性 Shell，用于存储对 ShellControl 的弱引用

    public static void RegisterRoute() // 定义静态方法 RegisterRoute
    {
        ServiceCollection serviceCollection = new(); // 创建 ServiceCollection 实例

        foreach (var pageItem in ShellPageSetting.PageDict) // 遍历 ShellPageSetting.PageDict 字典中的每个项目
        {
            serviceCollection.Register(pageItem.Key, pageItem.Value); // 将每个页面项注册到 serviceCollection
        }
        Provider = serviceCollection.BuildServiceProvider(); // 构建 ServiceProvider 并赋值给 Provider
    }

    public static FrameworkElement ResolveRoute(string route) // 定义静态方法 ResolveRoute，根据路由解析 FrameworkElement
    {
        if (string.IsNullOrEmpty(route)) // 如果路由为空或 null
        {
            return null!; // 返回 null
        }
        return Provider?.Resolve<FrameworkElement>(route)!; // 从 Provider 中解析 FrameworkElement
    }

    public static void GoTo(string route) // 定义静态方法 GoTo，根据路由切换页面
    {
        if (Shell != null) // 如果 Shell 不为空
        {
            if (Shell.TryGetTarget(out ShellControl shell)) // 尝试从 Shell 获取 ShellControl 实例
            {
                OnGoTo(route); // 执行 OnGoTo 方法
                shell.Content = ResolveRoute(route); // 设置 ShellControl 的内容为解析后的页面
                shell.Route = route; // 更新 ShellControl 的路由
            }
        }
    }

    public static void GoToNext() // 定义静态方法 GoToNext，切换到下一个页面
    {
        if (Shell != null) // 如果 Shell 不为空
        {
            if (Shell.TryGetTarget(out ShellControl shell)) // 尝试从 Shell 获取 ShellControl 实例
            {
                if (ShellPageSetting.PageDict.ContainsKey(shell.Route)) // 如果 ShellControl 的路由在 PageDict 中
                {
                    bool found = false; // 定义布尔变量 found，初始为 false
                    foreach (var item in ShellPageSetting.PageDict) // 遍历 PageDict 字典中的每个项目
                    {
                        if (found) // 如果 found 为 true
                        {
                            OnGoTo(item.Key); // 执行 OnGoTo 方法
                            shell.Content = ResolveRoute(item.Key); // 设置 ShellControl 的内容为解析后的页面
                            shell.Route = item.Key; // 更新 ShellControl 的路由
                            break; // 退出循环
                        }
                        if (item.Key == shell.Route) // 如果当前项的键等于 ShellControl 的路由
                        {
                            found = true; // 设置 found 为 true
                        }
                    }
                }
            }
        }
    }

    private static void OnGoTo(string route) // 定义私有静态方法 OnGoTo
    {
    }
}

file class RoutingServiceInfo // 定义文件级类 RoutingServiceInfo
{
    public string Name { get; set; } // 定义公共属性 Name
    public Type Type { get; set; } // 定义公共属性 Type

    public RoutingServiceInfo(string name, Type type) // 定义构造函数
    {
        Name = name; // 设置 Name 属性
        Type = type; // 设置 Type 属性
    }
}

file static class RoutingExtension // 定义文件级静态类 RoutingExtension
{
    public static IServiceCollection Register(this IServiceCollection services, string name, Type type) // 扩展方法，注册服务
    {
        services.AddSingleton(type); // 将类型添加为单例服务
        services.AddSingleton(new RoutingServiceInfo(name, type)); // 添加 RoutingServiceInfo 实例
        return services; // 返回 IServiceCollection
    }

    public static T Resolve<T>(this IServiceProvider serviceProvider, string name) // 扩展方法，从服务提供者中解析服务
    # 从服务提供者获取所有 RoutingServiceInfo 实例
    var serviceInfo = serviceProvider.GetRequiredService<IEnumerable<RoutingServiceInfo>>()
        # 查找名称匹配的第一个 RoutingServiceInfo 实例
        .FirstOrDefault(x => x.Name == name);

    # 如果未找到匹配的服务信息，则抛出异常
    if (serviceInfo == null)
    {
        throw new InvalidOperationException($"Service '{name}' not found");
    }
    # 根据服务信息中的类型获取所需服务，并强制转换为 T 类型
    return (T)serviceProvider.GetRequiredService(serviceInfo.Type);
# 代码块的结尾大括号，表示代码块的结束
}
```