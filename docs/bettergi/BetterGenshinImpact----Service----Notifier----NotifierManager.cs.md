# `.\better-genshin-impact\BetterGenshinImpact\Service\Notifier\NotifierManager.cs`

```cs
﻿using BetterGenshinImpact.Service.Notifier.Interface;  // 引用 Notifier.Interface 命名空间
using Microsoft.Extensions.Logging;  // 引用 Microsoft.Extensions.Logging 命名空间
using System.Collections.Generic;  // 引用 System.Collections.Generic 命名空间
using System.Diagnostics;  // 引用 System.Diagnostics 命名空间
using System.Linq;  // 引用 System.Linq 命名空间
using System.Net.Http;  // 引用 System.Net.Http 命名空间
using System.Threading.Tasks;  // 引用 System.Threading.Tasks 命名空间

namespace BetterGenshinImpact.Service.Notifier;  // 定义 BetterGenshinImpact.Service.Notifier 命名空间

public class NotifierManager
{
    // 私有字段，保存 INotifier 对象的列表
    private readonly List<INotifier> _notifiers = [];  // 初始化为空列表

    // 静态属性，获取 ILogger<NotifierManager> 的实例
    public static ILogger Logger { get; } = App.GetLogger<NotifierManager>();  // 使用 App.GetLogger 方法创建日志记录器

    // 构造函数
    public NotifierManager()
    {
    }

    // 注册一个新的 INotifier 实例到列表中
    public void RegisterNotifier(INotifier notifier)
    {
        _notifiers.Add(notifier);  // 将 INotifier 对象添加到 _notifiers 列表中
    }

    // 从列表中移除所有类型为 T 的 INotifier 实例
    public void RemoveNotifier<T>() where T : INotifier
    {
        _notifiers.RemoveAll(o => o is T);  // 移除所有符合类型 T 的 INotifier 对象
    }

    // 清空列表中的所有 INotifier 实例
    public void RemoveAllNotifiers()
    {
        _notifiers.Clear();  // 清空 _notifiers 列表
    }

    // 获取列表中第一个类型为 T 的 INotifier 实例
    public INotifier? GetNotifier<T>() where T : INotifier
    {
        return _notifiers.FirstOrDefault(o => o is T);  // 查找并返回第一个符合类型 T 的 INotifier 对象，若不存在则返回 null
    }

    // 异步发送通知，处理通知发送时的异常
    public async Task SendNotificationAsync(INotifier notifier, HttpContent httpContent)
    {
        try
        {
            await notifier.SendNotificationAsync(httpContent);  // 异步调用 INotifier 的 SendNotificationAsync 方法发送通知
        }
        catch (System.Exception ex)
        {
            Logger.LogError("{name} 通知发送失败", notifier.Name);  // 记录日志，通知发送失败
            Debug.WriteLine(ex);  // 将异常信息写入调试输出
        }
    }

    // 异步发送通知到指定类型的 INotifier 实例
    public async Task SendNotificationAsync<T>(HttpContent httpContent) where T : INotifier
    {
        var notifier = _notifiers.FirstOrDefault(o => o is T);  // 查找列表中第一个符合类型 T 的 INotifier 对象

        if (notifier != null)
        {
            await SendNotificationAsync(notifier, httpContent);  // 如果找到，调用 SendNotificationAsync 方法发送通知
        }
    }

    // 异步发送通知到所有注册的 INotifier 实例
    public async Task SendNotificationToAllAsync(HttpContent httpContent)
    {
        await Task.WhenAll(_notifiers.Select(notifier => SendNotificationAsync(notifier, httpContent)));  // 异步等待所有通知发送完成
    }
}
```