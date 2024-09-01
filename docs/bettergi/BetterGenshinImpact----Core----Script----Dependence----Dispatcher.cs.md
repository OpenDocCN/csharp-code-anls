# `.\better-genshin-impact\BetterGenshinImpact\Core\Script\Dependence\Dispatcher.cs`

```cs
# 引入依赖模型和命名空间
﻿using BetterGenshinImpact.Core.Script.Dependence.Model;
using BetterGenshinImpact.GameTask;
using System;

namespace BetterGenshinImpact.Core.Script.Dependence;

# 定义 Dispatcher 类
public class Dispatcher
{
    # 定义 RunTask 方法（当前未实现）
    public void RunTask()
    {
    }

    # 定义 AddTimer 方法，接受一个 RealtimeTimer 对象作为参数
    public void AddTimer(RealtimeTimer timer)
    {
        # 将传入的 timer 对象赋值给变量 realtimeTimer
        var realtimeTimer = timer;
        # 如果 realtimeTimer 为 null，抛出 ArgumentNullException 异常
        if (realtimeTimer == null)
        {
            throw new ArgumentNullException(nameof(realtimeTimer), "实时任务对象不能为空");
        }
        # 如果 realtimeTimer 的 Name 属性为空或 null，抛出 ArgumentNullException 异常
        if (string.IsNullOrEmpty(realtimeTimer.Name))
        {
            throw new ArgumentNullException(nameof(realtimeTimer.Name), "实时任务名称不能为空");
        }

        # 调用 TaskTriggerDispatcher 的单例实例，添加触发器
        TaskTriggerDispatcher.Instance().AddTrigger(realtimeTimer.Name, realtimeTimer.Config);
    }
}
```