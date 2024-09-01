# `.\better-genshin-impact\BetterGenshinImpact\Service\Notification\Builder\LifecycleNotificationBuilder.cs`

```cs
# 引入 BetterGenshinImpact.Service.Notification.Model 命名空间
﻿using BetterGenshinImpact.Service.Notification.Model;

# 引入 BetterGenshinImpact.Service.Notification.Builder 命名空间
namespace BetterGenshinImpact.Service.Notification.Builder;

# 定义一个类 LifecycleNotificationBuilder 实现 INotificationDataBuilder 接口，泛型参数为 LifecycleNotificationData
public class LifecycleNotificationBuilder : INotificationDataBuilder<LifecycleNotificationData>
{
    # 定义一个只读字段 _notificationData，初始化为一个新的 LifecycleNotificationData 实例
    private readonly LifecycleNotificationData _notificationData = new();

    # 实现 INotificationDataBuilder 接口的 Build 方法，返回 _notificationData
    public LifecycleNotificationData Build()
    {
        return _notificationData;
    }
}
```