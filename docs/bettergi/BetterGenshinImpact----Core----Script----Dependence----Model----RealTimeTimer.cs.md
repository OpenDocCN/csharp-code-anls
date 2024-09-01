# `.\better-genshin-impact\BetterGenshinImpact\Core\Script\Dependence\Model\RealTimeTimer.cs`

```cs
# 定义命名空间 BetterGenshinImpact.Core.Script.Dependence.Model
﻿namespace BetterGenshinImpact.Core.Script.Dependence.Model;

/// <summary>
/// 实时任务计时器
/// </summary>
public class RealtimeTimer
{
    /// <summary>
    /// 实时任务触发器名称
    /// </summary>
    public string? Name { get; set; }

    /// <summary>
    /// 实时任务触发器时间间隔
    /// 默认50ms
    /// </summary>
    public int Interval { get; set; } = 50;

    /// <summary>
    /// 实时任务配置
    /// </summary>
    public object? Config;

    # 构造函数：初始化 RealtimeTimer 类的新实例
    public RealtimeTimer()
    {
    }

    # 构造函数：使用指定名称初始化 RealtimeTimer 类的新实例
    public RealtimeTimer(string name)
    {
        Name = name;
    }
}
```