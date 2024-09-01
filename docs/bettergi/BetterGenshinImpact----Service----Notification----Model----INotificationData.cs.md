# `.\better-genshin-impact\BetterGenshinImpact\Service\Notification\Model\INotificationData.cs`

```cs
# 导入 GenshinImpact 服务通知模型中定义的枚举类型
﻿using BetterGenshinImpact.Service.Notification.Model.Enum;
# 导入 JSON 序列化和反序列化相关的功能
using System.Text.Json.Serialization;

# 定义命名空间 BetterGenshinImpact.Service.Notification.Model
namespace BetterGenshinImpact.Service.Notification.Model;

# 定义一个接口 INotificationData
public interface INotificationData
{
    # 指定属性 Event 应该用 JsonStringEnumConverter 进行 JSON 序列化和反序列化
    [JsonConverter(typeof(JsonStringEnumConverter))]
    # 声明一个公共属性 Event，其类型为 NotificationEvent
    public NotificationEvent Event { get; set; }
}
```