# `.\better-genshin-impact\BetterGenshinImpact\Service\Notification\Builder\TaskNotificationBuilder.cs`

```cs
# 引入需要的命名空间
﻿using BetterGenshinImpact.Service.Notification.Model;
using BetterGenshinImpact.Service.Notification.Model.Enum;
using System.Drawing;

# 定义命名空间
namespace BetterGenshinImpact.Service.Notification.Builder;

# 定义一个任务通知构建器类，实现 INotificationDataBuilder 接口
public class TaskNotificationBuilder : INotificationDataBuilder<TaskNotificationData>
{
    # 定义一个私有只读字段，保存通知数据
    private readonly TaskNotificationData _notificationData = new();

    # 设置通知事件，并返回当前构建器实例
    public TaskNotificationBuilder WithEvent(NotificationEvent notificationEvent)
    {
        _notificationData.Event = notificationEvent;
        return this;
    }

    # 设置通知动作，并返回当前构建器实例
    public TaskNotificationBuilder WithAction(NotificationAction notificationAction)
    {
        _notificationData.Action = notificationAction;
        return this;
    }

    # 设置通知结论（可选），并返回当前构建器实例
    public TaskNotificationBuilder WithConclusion(NotificationConclusion? conclusion)
    {
        _notificationData.Conclusion = conclusion;
        return this;
    }

    # 设置通知事件为 GeniusInvocation，并返回当前构建器实例
    public TaskNotificationBuilder GeniusInvocation()
    {
        return WithEvent(NotificationEvent.GeniusInvocation);
    }

    # 设置通知事件为 Domain，并返回当前构建器实例
    public TaskNotificationBuilder Domain()
    {
        return WithEvent(NotificationEvent.Domain);
    }

    # 设置通知动作为 Started，并返回当前构建器实例
    public TaskNotificationBuilder Started()
    {
        return WithAction(NotificationAction.Started);
    }

    # 设置通知动作为 Completed，并返回当前构建器实例
    public TaskNotificationBuilder Completed()
    {
        return WithAction(NotificationAction.Completed);
    }

    # 设置通知动作为 Progress，并返回当前构建器实例
    public TaskNotificationBuilder Progress()
    {
        return WithAction(NotificationAction.Progress);
    }

    # 设置通知动作为 Completed 并且结论为 Success，并返回当前构建器实例
    public TaskNotificationBuilder Success()
    {
        return WithAction(NotificationAction.Completed)
            .WithConclusion(NotificationConclusion.Success);
    }

    # 设置通知动作为 Completed 并且结论为 Failure，并返回当前构建器实例
    public TaskNotificationBuilder Failure()
    {
        return WithAction(NotificationAction.Completed)
            .WithConclusion(NotificationConclusion.Failure);
    }

    # 设置通知动作为 Completed 并且结论为 Cancelled，并返回当前构建器实例
    public TaskNotificationBuilder Cancelled()
    {
        return WithAction(NotificationAction.Completed)
            .WithConclusion(NotificationConclusion.Cancelled);
    }

    # 设置通知截图，并返回当前构建器实例
    public TaskNotificationBuilder WithScreenshot(Image? screenshot)
    {
        _notificationData.Screenshot = screenshot;
        return this;
    }

    # 添加任务对象，并返回当前构建器实例
    public TaskNotificationBuilder AddTask(object task)
    {
        _notificationData.Task = task;
        return this;
    }

    # 构建并返回通知数据对象
    public TaskNotificationData Build()
    {
        return _notificationData;
    }
}
```