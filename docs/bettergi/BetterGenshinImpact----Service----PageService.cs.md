# `.\better-genshin-impact\BetterGenshinImpact\Service\PageService.cs`

```cs
﻿using System;  // 引入 System 命名空间
using System.Windows;  // 引入 System.Windows 命名空间，以便使用 WPF 控件
using Wpf.Ui;  // 引入 Wpf.Ui 命名空间

namespace BetterGenshinImpact.Service  // 定义命名空间 BetterGenshinImpact.Service
{
    /// <summary>
    /// Service that provides pages for navigation.  // 提供页面导航服务的类
    /// </summary>
    public class PageService : IPageService  // 定义 PageService 类，实现 IPageService 接口
    {
        /// <summary>
        /// Service which provides the instances of pages.  // 提供页面实例的服务提供者
        /// </summary>
        private readonly IServiceProvider _serviceProvider;  // 定义只读字段 _serviceProvider 用于存储服务提供者

        /// <summary>
        /// Creates new instance and attaches the <see cref="IServiceProvider"/>.  // 创建新实例并附加 IServiceProvider
        /// </summary>
        public PageService(IServiceProvider serviceProvider)  // PageService 构造函数，接受 IServiceProvider 实例
        {
            _serviceProvider = serviceProvider;  // 将传入的服务提供者实例赋值给字段 _serviceProvider
        }

        /// <inheritdoc />  // 继承接口方法的文档注释
        public T? GetPage<T>() where T : class  // 泛型方法，获取指定类型的页面
        {
            if (!typeof(FrameworkElement).IsAssignableFrom(typeof(T)))  // 检查 T 类型是否是 FrameworkElement 的派生类
            {
                throw new InvalidOperationException("The page should be a WPF control.");  // 如果不是，则抛出异常
            }

            return (T?)_serviceProvider.GetService(typeof(T));  // 从服务提供者中获取 T 类型的实例并返回
        }

        /// <inheritdoc />  // 继承接口方法的文档注释
        public FrameworkElement? GetPage(Type pageType)  // 非泛型方法，根据类型获取页面
        {
            if (!typeof(FrameworkElement).IsAssignableFrom(pageType))  // 检查 pageType 是否是 FrameworkElement 的派生类
            {
                throw new InvalidOperationException("The page should be a WPF control.");  // 如果不是，则抛出异常
            }

            return _serviceProvider.GetService(pageType) as FrameworkElement;  // 从服务提供者中获取指定类型的实例并强制转换为 FrameworkElement
        }
    }
}
```