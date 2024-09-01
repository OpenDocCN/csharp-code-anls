# `.\better-genshin-impact\BetterGenshinImpact\GameTask\AutoWood\WoodTaskParam.cs`

```cs
﻿using BetterGenshinImpact.GameTask.Model; // 引入 BetterGenshinImpact.GameTask.Model 命名空间中的模型类
using System.Threading; // 引入 System.Threading 命名空间，以便使用多线程相关功能

namespace BetterGenshinImpact.GameTask.AutoWood; // 定义 BetterGenshinImpact.GameTask.AutoWood 命名空间

public class WoodTaskParam : BaseTaskParam // 定义一个名为 WoodTaskParam 的公共类，继承自 BaseTaskParam
{
    public int WoodRoundNum { get; set; } // 定义一个公共整数属性，用于存储木材任务的回合数
    public int WoodDailyMaxCount { get; set; } // 定义一个公共整数属性，用于存储木材每日最大数量

    // 定义构造函数，用于初始化 WoodTaskParam 实例
    public WoodTaskParam(CancellationTokenSource cts, int woodRoundNum, int woodDailyMaxCount) : base(cts)
    {
        WoodRoundNum = woodRoundNum; // 将传入的 woodRoundNum 参数赋值给 WoodRoundNum 属性
        if (woodRoundNum == 0) // 如果 woodRoundNum 为 0
        {
            WoodRoundNum = 9999; // 将 WoodRoundNum 属性设置为 9999
        }

        WoodDailyMaxCount = woodDailyMaxCount; // 将传入的 woodDailyMaxCount 参数赋值给 WoodDailyMaxCount 属性
        if (WoodDailyMaxCount is 0 or >= 2000) WoodDailyMaxCount = 2000; // 如果 WoodDailyMaxCount 为 0 或者大于等于 2000，将其设置为 2000
    }
}
```