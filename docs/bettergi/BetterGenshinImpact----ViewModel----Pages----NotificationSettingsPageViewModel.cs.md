# `.\better-genshin-impact\BetterGenshinImpact\ViewModel\Pages\NotificationSettingsPageViewModel.cs`

```cs
# 引入相关的命名空间和程序集
﻿using BetterGenshinImpact.Core.Config;
using BetterGenshinImpact.Service.Interface;
using BetterGenshinImpact.Service.Notification;
using BetterGenshinImpact.Service.Notifier;
using CommunityToolkit.Mvvm.ComponentModel;
using CommunityToolkit.Mvvm.Input;
using System.Threading.Tasks;

# 定义命名空间，用于组织 ViewModel 类
namespace BetterGenshinImpact.ViewModel.Pages;

# 定义一个部分类，继承自 ObservableObject 和 IViewModel
public partial class NotificationSettingsPageViewModel : ObservableObject, IViewModel
{
    # 定义一个公共属性，表示配置对象
    public AllConfig Config { get; set; }

    # 定义一个私有字段，用于存储通知服务实例
    private readonly NotificationService _notificationService;

    # 使用 ObservableProperty 特性来自动实现 INotifyPropertyChanged
    [ObservableProperty] private bool _isLoading;

    # 使用 ObservableProperty 特性来自动实现 INotifyPropertyChanged
    [ObservableProperty] private string _webhookStatus = string.Empty;

    # 构造函数，注入配置服务和通知服务实例
    public NotificationSettingsPageViewModel(IConfigService configService, NotificationService notificationService)
    {
        # 通过配置服务获取配置并赋值给 Config 属性
        Config = configService.Get();
        # 初始化通知服务实例
        _notificationService = notificationService;
    }

    # 使用 RelayCommand 特性来定义命令
    [RelayCommand]
    private async Task OnTestWebhook()
    {
        # 将 IsLoading 属性设置为 true，表示正在加载
        IsLoading = true;
        # 将 WebhookStatus 属性设置为空字符串，清除之前的状态
        WebhookStatus = string.Empty;

        # 异步调用通知服务的 TestNotifierAsync 方法测试 Webhook
        var res = await _notificationService.TestNotifierAsync<WebhookNotifier>();

        # 更新 WebhookStatus 属性为测试结果中的消息
        WebhookStatus = res.Message;

        # 将 IsLoading 属性设置为 false，表示加载完成
        IsLoading = false;
    }
}
```