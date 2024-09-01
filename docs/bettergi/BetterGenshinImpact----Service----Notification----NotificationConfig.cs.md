# `.\better-genshin-impact\BetterGenshinImpact\Service\Notification\NotificationConfig.cs`

```cs
# 引入 CommunityToolkit.Mvvm.ComponentModel 命名空间
using CommunityToolkit.Mvvm.ComponentModel;
# 引入 System 命名空间
using System;

# 定义 BetterGenshinImpact.Service.Notification 命名空间
namespace BetterGenshinImpact.Service.Notification
{
    # 定义一个可序列化的通知配置类，继承自 ObservableObject
    [Serializable]
    public partial class NotificationConfig : ObservableObject
    {
        # 定义一个布尔属性，用于指示 webhook 是否启用
        [ObservableProperty]
        private bool _webhookEnabled;

        # 定义一个字符串属性，用于存储 webhook 的端点
        [ObservableProperty]
        private string _webhookEndpoint = string.Empty;
    }
}
```