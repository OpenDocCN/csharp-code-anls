# `.\better-genshin-impact\BetterGenshinImpact\Service\Notifier\WebhookNotifier.cs`

```cs
# 引入自定义异常类
﻿using BetterGenshinImpact.Service.Notifier.Exception;
# 引入自定义接口
using BetterGenshinImpact.Service.Notifier.Interface;
# 引入 HTTP 客户端库
using System.Net.Http;
# 引入异步编程库
using System.Threading.Tasks;

# 命名空间定义
namespace BetterGenshinImpact.Service.Notifier;

# 定义一个实现了 INotifier 接口的 WebhookNotifier 类
public class WebhookNotifier : INotifier
{
    # 属性 Name，默认为 "Webhook"
    public string Name { get; set; } = "Webhook";

    # 属性 Endpoint，存储 Webhook 的目标地址
    public string Endpoint { get; set; }

    # 私有字段 _httpClient，用于发送 HTTP 请求
    private readonly HttpClient _httpClient;

    # 构造函数，初始化 _httpClient 和 Endpoint 属性
    public WebhookNotifier(HttpClient httpClient, string endpoint = "")
    {
        _httpClient = httpClient;
        Endpoint = endpoint;
    }

    # 异步方法，发送通知
    public async Task SendNotificationAsync(HttpContent content)
    {
        try
        {
            # 使用 _httpClient 发送 POST 请求到 Endpoint，包含指定内容
            var response = await _httpClient.PostAsync(Endpoint, content);

            # 如果响应状态码不是成功状态码，抛出自定义异常
            if (!response.IsSuccessStatusCode)
            {
                throw new NotifierException($"Webhook call failed with code: {response.StatusCode}");
            }
        }
        catch (NotifierException)
        {
            # 捕获并重新抛出 NotifierException 异常
            throw;
        }
        catch (System.Exception ex)
        {
            # 捕获其他异常并抛出带有异常信息的 NotifierException
            throw new NotifierException($"Error sending webhook: {ex.Message}");
        }
    }
}
```