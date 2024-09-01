# `.\better-genshin-impact\BetterGenshinImpact\Service\Notification\Model\NotificationTestResult.cs`

```cs
# 定义命名空间
﻿namespace BetterGenshinImpact.Service.Notification.Model;

# 定义 NotificationTestResult 类
public class NotificationTestResult
{
    # 表示操作是否成功的属性
    public bool IsSuccess { get; set; }
    # 存储消息的属性，默认值为空字符串
    public string Message { get; set; } = string.Empty;

    # 静态方法，创建成功的通知结果
    public static NotificationTestResult Success()
    {
        # 返回一个新的 NotificationTestResult 对象，表示成功
        return new NotificationTestResult { IsSuccess = true, Message = "成功" };
    }

    # 静态方法，创建带有错误消息的通知结果
    public static NotificationTestResult Error(string message)
    {
        # 返回一个新的 NotificationTestResult 对象，表示失败并包含错误消息
        return new NotificationTestResult { IsSuccess = false, Message = message };
    }
}
```