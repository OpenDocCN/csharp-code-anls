# `.\better-genshin-impact\BetterGenshinImpact\Service\Notification\Model\LifecycleNotificationData.cs`

```cs
# 使用 BetterGenshinImpact.Service.Notification.Model.Enum 命名空间中的类型
﻿using BetterGenshinImpact.Service.Notification.Model.Enum;
# 使用 System.Text.Json.Serialization 命名空间中的 JsonStringEnumConverter 类型
using System.Text.Json.Serialization;

# 定义 BetterGenshinImpact.Service.Notification.Model 命名空间
namespace BetterGenshinImpact.Service.Notification.Model;

# 定义一个记录类型 LifecycleNotificationData，继承自 INotificationData 接口
public record LifecycleNotificationData : INotificationData
{
    # 为 Event 属性指定 JsonStringEnumConverter 以支持 JSON 字符串到枚举的转换
    [JsonConverter(typeof(JsonStringEnumConverter))]
    # 定义 Event 属性，类型为 NotificationEvent
    public NotificationEvent Event { get; set; }

    # 定义一个静态方法 Test 返回一个 LifecycleNotificationData 实例
    public static LifecycleNotificationData Test()
    {
        # 返回一个 LifecycleNotificationData 实例，Event 属性设置为 NotificationEvent.Test
        return new LifecycleNotificationData() { Event = NotificationEvent.Test };
    }
}
```