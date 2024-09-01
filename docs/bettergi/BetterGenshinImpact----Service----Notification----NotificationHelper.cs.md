# `.\better-genshin-impact\BetterGenshinImpact\Service\Notification\NotificationHelper.cs`

```cs
# 引用 BetterGenshinImpact.GameTask.Common 命名空间
﻿using BetterGenshinImpact.GameTask.Common;
# 引用 BetterGenshinImpact.Service.Notification.Builder 命名空间
using BetterGenshinImpact.Service.Notification.Builder;
# 引用 BetterGenshinImpact.Service.Notification.Model 命名空间
using BetterGenshinImpact.Service.Notification.Model;
# 引用 System 命名空间
using System;
# 引用 System.Drawing 命名空间
using System.Drawing;

# 定义 BetterGenshinImpact.Service.Notification 命名空间
namespace BetterGenshinImpact.Service.Notification
{
    # 定义 NotificationHelper 类
    public class NotificationHelper
    {
        # 定义静态方法 Notify，接受一个 INotificationData 参数
        public static void Notify(INotificationData notificationData)
        {
            # 调用 NotificationService 实例的 NotifyAllNotifiers 方法，通知所有通知器
            NotificationService.Instance().NotifyAllNotifiers(notificationData);
        }

        # 定义静态方法 SendTaskNotificationUsing，接受一个 Func<TaskNotificationBuilder, INotificationData> 委托
        public static void SendTaskNotificationUsing(Func<TaskNotificationBuilder, INotificationData> builderFunc)
        {
            # 创建一个 TaskNotificationBuilder 实例
            var builder = new TaskNotificationBuilder();
            # 调用 Notify 方法，传入 builderFunc(builder) 的结果
            Notify(builderFunc(builder));
        }

        # 定义静态方法 SendTaskNotificationWithScreenshotUsing，接受一个 Func<TaskNotificationBuilder, INotificationData> 委托
        public static void SendTaskNotificationWithScreenshotUsing(Func<TaskNotificationBuilder, INotificationData> builderFunc)
        {
            # 创建一个 TaskNotificationBuilder 实例
            var builder = new TaskNotificationBuilder();
            # 捕获屏幕截图并克隆为 Bitmap 对象
            var screenShot = (Bitmap)TaskControl.CaptureToRectArea().SrcBitmap.Clone();
            # 调用 Notify 方法，传入 builderFunc(builder.WithScreenshot(screenShot)) 的结果
            Notify(builderFunc(builder.WithScreenshot(screenShot)));
        }
    }
}
```