# `.\better-genshin-impact\BetterGenshinImpact\Service\Notifier\Interface\INotifier.cs`

```cs
# 引入用于处理 HTTP 请求的命名空间
﻿using System.Net.Http;
# 引入用于异步操作的命名空间
using System.Threading.Tasks;

# 定义一个命名空间，组织相关的代码
namespace BetterGenshinImpact.Service.Notifier.Interface;

# 定义一个接口 INotifier，用于通知服务的实现
public interface INotifier
{
    # 定义一个只读属性 Name，获取通知器的名称
    string Name { get; }

    # 定义一个异步方法 SendNotificationAsync，用于发送通知
    # TODO: 使用其他数据结构替代 HttpContent
    Task SendNotificationAsync(HttpContent content);
}
```