# `.\better-genshin-impact\BetterGenshinImpact\GameTask\AutoGeniusInvokation\AutoGeniusInvokationTask.cs`

```cs
# 引入 BetterGenshinImpact.Helpers.Extensions 命名空间中的扩展方法
﻿using BetterGenshinImpact.Helpers.Extensions;

# 定义 BetterGenshinImpact.GameTask.AutoGeniusInvokation 命名空间
namespace BetterGenshinImpact.GameTask.AutoGeniusInvokation;

# 定义 AutoGeniusInvokationTask 类
public class AutoGeniusInvokationTask
{
    # 定义静态方法 Start，接收 GeniusInvokationTaskParam 类型的 taskParam 参数
    public static void Start(GeniusInvokationTaskParam taskParam)
    {
        # 停止任务触发调度器的计时器
        TaskTriggerDispatcher.Instance().StopTimer();
        # 解析策略内容，生成对应的 duel 对象
        var duel = ScriptParser.Parse(taskParam.StrategyContent);
        # 激活系统控制窗口
        SystemControl.ActivateWindow();
        # 异步运行 duel 对象，任务完成后不会等待结果
        duel.RunAsync(taskParam).SafeForget();
    }
}
```