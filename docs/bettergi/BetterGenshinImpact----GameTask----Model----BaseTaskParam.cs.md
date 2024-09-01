# `.\better-genshin-impact\BetterGenshinImpact\GameTask\Model\BaseTaskParam.cs`

```cs
﻿using BetterGenshinImpact.GameTask.Model.Enum;  // 导入 BetterGenshinImpact.GameTask.Model.Enum 命名空间
using System.Threading;  // 导入 System.Threading 命名空间

namespace BetterGenshinImpact.GameTask.Model;  // 定义 BetterGenshinImpact.GameTask.Model 命名空间

/// <summary>
/// 独立任务参数基类
/// </summary>
public class BaseTaskParam
{
    public string Name { get; set; } = string.Empty;  // 任务的名称，默认初始化为空字符串

    public CancellationTokenSource Cts { get; set; }  // 用于任务取消的取消令牌源

    /// <summary>
    /// 针对实时触发器的操作
    /// </summary>
    public DispatcherTimerOperationEnum TriggerOperation { get; set; } = DispatcherTimerOperationEnum.None;  // 实时触发器的操作类型，默认设置为 None

    protected BaseTaskParam(CancellationTokenSource cts)  // 构造函数，接收取消令牌源作为参数
    {
        this.Cts = cts;  // 初始化 Cts 属性
    }
}
```