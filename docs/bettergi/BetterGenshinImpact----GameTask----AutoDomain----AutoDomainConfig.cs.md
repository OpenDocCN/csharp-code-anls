# `.\better-genshin-impact\BetterGenshinImpact\GameTask\AutoDomain\AutoDomainConfig.cs`

```cs
﻿using CommunityToolkit.Mvvm.ComponentModel; // 引入 CommunityToolkit.Mvvm.ComponentModel 命名空间，提供 MVVM 支持
using System; // 引入 System 命名空间，提供基本的系统功能

namespace BetterGenshinImpact.GameTask.AutoDomain; // 定义命名空间 BetterGenshinImpact.GameTask.AutoDomain

[Serializable] // 指定类可以被序列化
public partial class AutoDomainConfig : ObservableObject // 定义 AutoDomainConfig 类，继承自 ObservableObject
{

    /// <summary>
    /// 战斗结束后延迟几秒再开始寻找石化古树，秒
    /// </summary>
    [ObservableProperty] private double _fightEndDelay = 5; // 定义一个私有的 double 类型字段 _fightEndDelay，默认为 5 秒，并且属性具备通知功能

    /// <summary>
    /// 寻找古树时，短距离移动，用于识别速度过慢的计算机使用
    /// </summary>
    [ObservableProperty] private bool _shortMovement = false; // 定义一个私有的 bool 类型字段 _shortMovement，默认为 false，并且属性具备通知功能

    /// <summary>
    /// 寻找古树时，短距离移动，用于识别速度过慢的计算机使用
    /// </summary>
    [ObservableProperty] private bool _walkToF = false; // 定义一个私有的 bool 类型字段 _walkToF，默认为 false，并且属性具备通知功能

    /// <summary>
    /// 寻找古树时，短距离移动的次数
    /// </summary>
    [ObservableProperty] private int _leftRightMoveTimes = 3; // 定义一个私有的 int 类型字段 _leftRightMoveTimes，默认为 3 次，并且属性具备通知功能
}
```