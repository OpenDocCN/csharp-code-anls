# `.\better-genshin-impact\BetterGenshinImpact\Core\Script\Dependence\Log.cs`

```cs
# 引入 Microsoft.Extensions.Logging 命名空间，用于日志记录
﻿using Microsoft.Extensions.Logging;

# 定义 BetterGenshinImpact.Core.Script.Dependence 命名空间
namespace BetterGenshinImpact.Core.Script.Dependence;

# 定义 Log 类
public class Log
{
    # 创建一个私有的 ILogger<Log> 类型的字段，通过 App.GetLogger<Log>() 方法获取日志记录器
    private readonly ILogger<Log> _logger = App.GetLogger<Log>();

    # 定义 Debug 方法，用于记录调试信息
    public void Debug(string? message, params object?[] args)
    {
        # 使用 ILogger<Log> 记录信息级别的日志，传入消息和参数
        _logger.LogInformation(message, args);
    }

    # 定义 Info 方法，用于记录普通信息
    public void Info(string? message, params object?[] args)
    {
        # 使用 ILogger<Log> 记录信息级别的日志，传入消息和参数
        _logger.LogInformation(message, args);
    }

    # 定义 Warn 方法，用于记录警告信息
    public void Warn(string? message, params object?[] args)
    {
        # 使用 ILogger<Log> 记录警告级别的日志，传入消息和参数
        _logger.LogWarning(message, args);
    }

    # 定义 Error 方法，用于记录错误信息
    public void Error(string? message, params object?[] args)
    {
        # 使用 ILogger<Log> 记录错误级别的日志，传入消息和参数
        _logger.LogError(message, args);
    }
}
```