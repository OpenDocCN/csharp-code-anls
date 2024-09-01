# `.\better-genshin-impact\BetterGenshinImpact\Service\Notification\NotificationService.cs`

```cs
# 命名空间，用于包含与通知相关的服务和模型
namespace BetterGenshinImpact.Service.Notification;

# 定义一个实现 IHostedService 接口的通知服务类
public class NotificationService : IHostedService
{
    # 静态变量，单例模式，保存 NotificationService 实例
    private static NotificationService? _instance;

    # 静态只读 HttpClient 实例，用于发送 HTTP 请求
    private static readonly HttpClient _httpClient = new();
    
    # 私有字段，用于管理通知者
    private readonly NotifierManager _notifierManager;
    
    # 属性，存储配置数据
    private AllConfig Config { get; set; }

    # 构造函数，初始化配置和通知者管理器
    public NotificationService(IConfigService configService, NotifierManager notifierManager)
    {
        # 从配置服务获取配置数据
        Config = configService.Get();
        
        # 设置通知者管理器
        _notifierManager = notifierManager;
        
        # 设置单例实例
        _instance = this;
        
        # 初始化通知者
        InitializeNotifiers();
    }

    # 静态方法，获取 NotificationService 的实例
    public static NotificationService Instance()
    {
        # 如果实例为空，则抛出异常
        if (_instance == null)
        {
            throw new Exception("Not instantiated");
        }
        # 返回单例实例
        return _instance;
    }

    # 实现 IHostedService 接口的方法，启动服务时的操作
    public Task StartAsync(CancellationToken cancellationToken)
    {
        # 目前不做任何操作，直接返回已完成的任务
        return Task.CompletedTask;
    }

    # 实现 IHostedService 接口的方法，停止服务时的操作
    public Task StopAsync(CancellationToken cancellationToken)
    {
        # 目前不做任何操作，直接返回已完成的任务
        return Task.CompletedTask;
    }

    # 将通知数据转换为 JSON 格式的 StringContent
    private StringContent TransformData(INotificationData notificationData)
    {
        # 序列化通知数据对象为 JSON 字符串，使用驼峰命名策略
        var serializedData = JsonSerializer.Serialize<object>(notificationData, new JsonSerializerOptions
        {
            PropertyNamingPolicy = JsonNamingPolicy.CamelCase,
        });

        # 创建 StringContent 对象，设置编码为 UTF-8，媒体类型为 application/json
        return new StringContent(serializedData, Encoding.UTF8, "application/json");
    }

    # 初始化通知者，根据配置启用 Webhook 通知者
    private void InitializeNotifiers()
    {
        # 如果配置中启用了 Webhook
        if (Config.NotificationConfig.WebhookEnabled)
        {
            # 注册 Webhook 通知者
            _notifierManager.RegisterNotifier(new WebhookNotifier(_httpClient, Config.NotificationConfig.WebhookEndpoint));
        }
    }

    # 刷新通知者，移除所有已注册的通知者并重新初始化
    public void RefreshNotifiers()
    {
        # 移除所有已注册的通知者
        _notifierManager.RemoveAllNotifiers();
        
        # 重新初始化通知者
        InitializeNotifiers();
    }

    # 异步测试通知者，检查是否能成功发送通知
    public async Task<NotificationTestResult> TestNotifierAsync<T>() where T : INotifier
    {
        try
        {
            # 从通知者管理器获取指定类型的通知者
            var notifier = _notifierManager.GetNotifier<T>();
            
            # 如果通知者为空，返回错误结果
            if (notifier == null)
            {
                return NotificationTestResult.Error("通知类型未启用");
            }
            
            # 发送测试通知
            await notifier.SendNotificationAsync(TransformData(LifecycleNotificationData.Test()));
            
            # 返回成功结果
            return NotificationTestResult.Success();
        }
        catch (NotifierException ex)
        {
            # 捕获通知者异常，返回错误结果
            return NotificationTestResult.Error(ex.Message);
        }
    }

    # 异步通知所有通知者
    public async Task NotifyAllNotifiersAsync(INotificationData notificationData)
    # 异步发送通知给所有通知管理器，发送的数据是经过 TransformData 转换后的结果
    {
        await _notifierManager.SendNotificationToAllAsync(TransformData(notificationData));
    }

    # 启动一个新的任务来异步通知所有通知管理器
    public void NotifyAllNotifiers(INotificationData notificationData)
    {
        # 在新线程中运行 NotifyAllNotifiersAsync 方法，以避免阻塞当前线程
        Task.Run(() => NotifyAllNotifiersAsync(notificationData));
    }
请提供需要注释的代码片段。
```