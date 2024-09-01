# `.\better-genshin-impact\BetterGenshinImpact\GameTask\AutoFight\AutoFightParam.cs`

```cs
# 引入 BetterGenshinImpact.GameTask.Model 命名空间
﻿using BetterGenshinImpact.GameTask.Model;
# 引入 System.Threading 命名空间
using System.Threading;

# 定义命名空间 BetterGenshinImpact.GameTask.AutoFight
namespace BetterGenshinImpact.GameTask.AutoFight;

# 定义 AutoFightParam 类，继承自 BaseTaskParam
public class AutoFightParam : BaseTaskParam
{
    # 定义一个公开的字符串属性 CombatStrategyPath
    public string CombatStrategyPath { get; set; }

    # 定义构造函数，接受 CancellationTokenSource 和路径作为参数
    public AutoFightParam(CancellationTokenSource cts, string path) : base(cts)
    {
        # 初始化 CombatStrategyPath 属性
        CombatStrategyPath = path;
    }
}
```