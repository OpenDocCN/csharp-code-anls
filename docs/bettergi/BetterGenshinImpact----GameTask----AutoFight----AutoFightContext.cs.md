# `.\better-genshin-impact\BetterGenshinImpact\GameTask\AutoFight\AutoFightContext.cs`

```cs
﻿using BetterGenshinImpact.Core.Simulator;  // 引入 Core.Simulator 命名空间
using BetterGenshinImpact.GameTask.AutoFight.Assets;  // 引入 GameTask.AutoFight.Assets 命名空间
using BetterGenshinImpact.Model;  // 引入 Model 命名空间

namespace BetterGenshinImpact.GameTask.AutoFight;  // 定义命名空间 BetterGenshinImpact.GameTask.AutoFight

/// <summary>
/// 自动战斗上下文  // 类的描述，说明这是一个用于自动战斗的上下文
/// 请在启动BetterGI以后再初始化  // 说明初始化这个上下文的条件
/// </summary>
public class AutoFightContext : Singleton<AutoFightContext>  // 定义 AutoFightContext 类，继承自 Singleton<AutoFightContext> 单例模式
{
    private AutoFightContext()  // 构造函数，私有以防止外部实例化
    {
        // 初始化 Simulator 字段为一个新的 PostMessageSimulator 实例，传入当前游戏句柄
        Simulator = Simulation.PostMessage(TaskContext.Instance().GameHandle);  
    }

    /// <summary>
    /// find资源  // 属性的描述，说明它用于查找资源
    /// </summary>
    public AutoFightAssets FightAssets => AutoFightAssets.Instance;  // 属性 FightAssets 返回 AutoFightAssets 类的单例实例

    /// <summary>
    /// 战斗专用的PostMessage模拟键鼠操作  // 属性的描述，说明它用于模拟键鼠操作
    /// </summary>
    public readonly PostMessageSimulator Simulator;  // 只读字段，保存 PostMessageSimulator 的实例，用于战斗中的键鼠操作模拟
}
```