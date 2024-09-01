# `.\better-genshin-impact\BetterGenshinImpact\Service\Notification\Model\TaskNotificationData.cs`

```cs
# 使用 JSON 字符串转换器来处理枚举和其他类型的序列化和反序列化
using BetterGenshinImpact.Service.Notification.Converter;
using BetterGenshinImpact.Service.Notification.Model.Enum;
using System.Drawing;
using System.Text.Json.Serialization;

# 定义命名空间，组织代码
namespace BetterGenshinImpact.Service.Notification.Model;

# 定义一个记录类型 TaskNotificationData，实现 INotificationData 接口
public record TaskNotificationData : INotificationData
{
    # 使用 JsonStringEnumConverter 将枚举类型转换为 JSON 字符串
    [JsonConverter(typeof(JsonStringEnumConverter))]
    public NotificationEvent Event { get; set; }  # 事件类型

    # 使用 JsonStringEnumConverter 将枚举类型转换为 JSON 字符串
    [JsonConverter(typeof(JsonStringEnumConverter))]
    public NotificationAction Action { get; set; }  # 操作类型

    # 使用 JsonStringEnumConverter 将枚举类型转换为 JSON 字符串，允许为 null
    [JsonConverter(typeof(JsonStringEnumConverter))]
    public NotificationConclusion? Conclusion { get; set; }  # 结论类型，可能为 null

    # 使用 ImageToBase64Converter 将图像转换为 Base64 编码的 JSON 字符串
    [JsonConverter(typeof(ImageToBase64Converter))]
    public Image? Screenshot { get; set; }  # 截图图像，可能为 null

    # 任务对象，可能为 null
    public object? Task { get; set; }  
}
```