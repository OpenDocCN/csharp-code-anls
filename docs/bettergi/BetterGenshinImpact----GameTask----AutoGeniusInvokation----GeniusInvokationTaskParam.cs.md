# `.\better-genshin-impact\BetterGenshinImpact\GameTask\AutoGeniusInvokation\GeniusInvokationTaskParam.cs`

```cs
# 引入 BetterGenshinImpact.GameTask.Model 命名空间
﻿using BetterGenshinImpact.GameTask.Model;
# 引入 System.Threading 命名空间
using System.Threading;

# 声明 BetterGenshinImpact.GameTask.AutoGeniusInvokation 命名空间
namespace BetterGenshinImpact.GameTask.AutoGeniusInvokation;

# 定义 GeniusInvokationTaskParam 类，继承自 BaseTaskParam
public class GeniusInvokationTaskParam(CancellationTokenSource cts, string strategyContent) : BaseTaskParam(cts)
{
    # 声明 StrategyContent 属性，并初始化为传入的 strategyContent 参数
    public string StrategyContent { get; set; } = strategyContent;
}
```