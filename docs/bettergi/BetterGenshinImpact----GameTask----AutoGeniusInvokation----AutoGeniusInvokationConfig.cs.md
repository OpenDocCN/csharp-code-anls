# `.\better-genshin-impact\BetterGenshinImpact\GameTask\AutoGeniusInvokation\AutoGeniusInvokationConfig.cs`

```cs
﻿using CommunityToolkit.Mvvm.ComponentModel; // 引入 CommunityToolkit 的 MVVM 组件模型
using OpenCvSharp; // 引入 OpenCvSharp 库，用于计算机视觉
using System; // 引入 System 命名空间
using System.Collections.Generic; // 引入 System.Collections.Generic 命名空间，包含泛型集合

namespace BetterGenshinImpact.GameTask.AutoGeniusInvokation; // 定义命名空间

/// <summary>
/// 自动打牌配置
/// </summary>
[Serializable] // 标记该类可以被序列化
public partial class AutoGeniusInvokationConfig : ObservableObject
{
    [ObservableProperty] private string _strategyName = "1.莫娜砂糖琴"; // 定义策略名称属性，初始化为 "1.莫娜砂糖琴"

    [ObservableProperty] private int _sleepDelay = 0; // 定义睡眠延迟属性，初始化为 0

    public List<Rect> DefaultCharacterCardRects { get; set; } = // 定义角色卡牌区域的默认矩形列表
    [
        new(667, 632, 165, 282), // 第一个角色卡牌区域的矩形
        new(877, 632, 165, 282), // 第二个角色卡牌区域的矩形
        new(1088, 632, 165, 282) // 第三个角色卡牌区域的矩形
    ];


    /// <summary>
    /// 骰子数量文字识别区域
    /// </summary>
    public Rect MyDiceCountRect { get; } = new(68, 642, 25, 31); // 定义骰子数量文字识别区域的矩形

    ///// <summary>
    ///// 角色卡牌区域向左扩展距离，包含HP区域
    ///// </summary>
    //public int CharacterCardLeftExtend { get; } = 20; // 角色卡牌区域向左扩展距离

    ///// <summary>
    ///// 角色卡牌区域向右扩展距离，包含充能区域
    ///// </summary>
    //public int CharacterCardRightExtend { get; } = 14; // 角色卡牌区域向右扩展距离

    /// <summary>
    /// 出战角色卡牌区域向上或者向下的距离差
    /// </summary>
    public int ActiveCharacterCardSpace { get; set; } = 41; // 定义出战角色卡牌区域的距离差，初始化为 41

    /// <summary>
    /// HP区域 在 角色卡牌区域 的相对位置
    /// </summary>
    public Rect CharacterCardExtendHpRect { get; } = new(-20, 0, 60, 55); // 定义 HP 区域在角色卡牌区域中的相对位置矩形
}
```