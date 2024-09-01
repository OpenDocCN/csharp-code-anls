# `.\better-genshin-impact\BetterGenshinImpact\Core\Config\MacroConfig.cs`

```cs
﻿using BetterGenshinImpact.GameTask.AutoFight; // 引入 BetterGenshinImpact.GameTask.AutoFight 命名空间，用于访问与自动战斗相关的功能
using CommunityToolkit.Mvvm.ComponentModel; // 引入 CommunityToolkit.Mvvm.ComponentModel 命名空间，用于实现 MVVM 模式中的通知属性功能
using System; // 引入 System 命名空间，用于访问基本的系统功能

namespace BetterGenshinImpact.Core.Config; // 定义 BetterGenshinImpact.Core.Config 命名空间，用于组织配置相关的代码

[Serializable] // 标记该类可以被序列化
public partial class MacroConfig : ObservableObject // 定义 MacroConfig 类，继承自 ObservableObject，以便于在属性更改时通知视图
{
    /// <summary>
    ///     高延迟下强化的额外等待时间
    ///     https://github.com/babalae/better-genshin-impact/issues/9
    /// </summary>
    [ObservableProperty] // 标记此属性为可观察的，以便在属性值更改时通知绑定到它的视图
    private int _enhanceWaitDelay; // 定义用于存储高延迟下强化的额外等待时间的私有字段

    /// <summary>
    ///     F连发时间间隔
    /// </summary>
    [ObservableProperty] // 标记此属性为可观察的
    private int _fFireInterval = 100; // 定义用于存储 F 连发时间间隔的私有字段，初始值为 100

    /// <summary>
    ///     长按F变F连发
    /// </summary>
    [ObservableProperty] // 标记此属性为可观察的
    private bool _fPressHoldToContinuationEnabled; // 定义用于存储长按 F 是否变为 F 连发的私有字段

    /// <summary>
    ///     转圈圈时间间隔
    /// </summary>
    [ObservableProperty] // 标记此属性为可观察的
    private int _runaroundInterval = 10; // 定义用于存储转圈圈时间间隔的私有字段，初始值为 10

    /// <summary>
    ///     转圈圈鼠标右移长度
    /// </summary>
    [ObservableProperty] // 标记此属性为可观察的
    private int _runaroundMouseXInterval = 500; // 定义用于存储转圈圈鼠标右移长度的私有字段，初始值为 500

    /// <summary>
    ///     空格连发时间间隔
    /// </summary>
    [ObservableProperty] // 标记此属性为可观察的
    private int _spaceFireInterval = 100; // 定义用于存储空格连发时间间隔的私有字段，初始值为 100

    /// <summary>
    ///     长按空格变空格连发
    /// </summary>
    [ObservableProperty] // 标记此属性为可观察的
    private bool _spacePressHoldToContinuationEnabled; // 定义用于存储长按空格是否变为空格连发的私有字段

    /// <summary>
    ///     一键战斗宏启用状态
    /// </summary>
    [ObservableProperty] // 标记此属性为可观察的
    private bool _combatMacroEnabled; // 定义用于存储一键战斗宏启用状态的私有字段

    /// <summary>
    ///     一键战斗宏快捷键模式
    /// </summary>
    [ObservableProperty] // 标记此属性为可观察的
    private string _combatMacroHotkeyMode = OneKeyFightTask.HoldOnMode; // 定义用于存储一键战斗宏快捷键模式的私有字段，初始值为 OneKeyFightTask.HoldOnMode

    /// <summary>
    ///     一键战斗宏优先级
    /// </summary>
    [ObservableProperty] // 标记此属性为可观察的
    private int _combatMacroPriority = 1; // 定义用于存储一键战斗宏优先级的私有字段，初始值为 1
}
```