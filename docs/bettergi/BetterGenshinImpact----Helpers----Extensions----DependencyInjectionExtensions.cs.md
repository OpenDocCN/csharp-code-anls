# `.\better-genshin-impact\BetterGenshinImpact\Helpers\Extensions\DependencyInjectionExtensions.cs`

```cs
# 引入命名空间，用于实现依赖注入
﻿using BetterGenshinImpact.ViewModel;
using Microsoft.Extensions.DependencyInjection;
using System.Windows.Controls;

# 定义命名空间
namespace BetterGenshinImpact.Helpers.Extensions;

# 定义一个静态类，用于扩展服务集合的依赖注入功能
internal static class DependencyInjectionExtensions
{
    # 定义一个扩展方法，添加窗口和视图模型的服务到服务集合
    public static IServiceCollection AddView<TWindow, TWindowImplementation, TViewModel>(this IServiceCollection services)
        where TWindow : class
        where TWindowImplementation : class, TWindow
        where TViewModel : class, IViewModel
    {
        # 将窗口和视图模型的实现类添加为单例服务
        return services
            .AddSingleton<TWindow, TWindowImplementation>()
            .AddSingleton<TViewModel>();
    }

    # 定义另一个扩展方法，添加页面和视图模型的服务到服务集合
    public static IServiceCollection AddView<TPage, TViewModel>(this IServiceCollection services)
        where TPage : Page
        where TViewModel : class, IViewModel
    {
        # 将页面和视图模型的实现类添加为单例服务
        return services
            .AddSingleton<TPage>()
            .AddSingleton<TViewModel>();
    }
}
```