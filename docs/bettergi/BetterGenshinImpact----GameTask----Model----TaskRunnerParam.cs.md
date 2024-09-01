# `.\better-genshin-impact\BetterGenshinImpact\GameTask\Model\TaskRunnerParam.cs`

```cs
﻿using BetterGenshinImpact.GameTask.Model.Enum; // 引入 BetterGenshinImpact.GameTask.Model.Enum 命名空间
using System.Threading; // 引入 System.Threading 命名空间

namespace BetterGenshinImpact.GameTask.Model; // 定义 BetterGenshinImpact.GameTask.Model 命名空间

/// <summary>
/// 独立任务参数基类
/// </summary>
public class TaskRunnerParam
{
    public string Name { get; set; } = string.Empty; // 定义一个字符串属性 Name，初始化为空字符串

    public CancellationTokenSource Cts { get; set; } // 定义一个 CancellationTokenSource 类型的属性 Cts

    /// <summary>
    /// 针对实时触发器的操作
    /// </summary>
    public DispatcherTimerOperationEnum TriggerOperation { get; set; } = DispatcherTimerOperationEnum.None; // 定义一个 DispatcherTimerOperationEnum 类型的属性 TriggerOperation，初始化为 None

    public bool UseLock { get; set; } // 定义一个布尔型属性 UseLock

    public TaskRunnerParam(CancellationTokenSource cts) // 定义构造函数，接受一个 CancellationTokenSource 类型的参数
    {
        this.Cts = cts; // 将传入的 CancellationTokenSource 参数赋值给属性 Cts
    }
}
```