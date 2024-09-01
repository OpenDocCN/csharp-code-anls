# `.\better-genshin-impact\BetterGenshinImpact\GameTask\AutoTrackPath\Model\GiWorldPosition.cs`

```cs
﻿namespace BetterGenshinImpact.GameTask.AutoTrackPath.Model; // 定义命名空间

/// <summary>
/// 原神世界坐标
/// https://github.com/babalae/better-genshin-impact/issues/318
/// </summary>
public class GiWorldPosition
{
    /// <summary>
    /// 为这个坐标点命名
    /// </summary>
    public string? Name { get; set; } // 坐标点名称

    /// <summary>
    /// 坐标描述
    /// </summary>
    public string? Description { get; set; } // 坐标描述

    /// <summary>
    /// 坐标 x,y,z 三个值，分别代表纵向、高度、横向，采用原神实际的坐标系
    /// 由于这个坐标系和一般的坐标系不同，所以为了方便理解，设这3个值为a,b,c
    ///     ▲
    ///     │a
    /// ◄───┼────
    ///   c │
    ///
    /// 值的缩放等级和1024区块坐标系的缩放一致
    /// </summary>
    public decimal[] Position { get; set; } = new decimal[3]; // 坐标数组，默认为包含三个 decimal 元素的数组

    public double X => (double)Position[2]; // c // 将 Position 数组的第三个值转换为 double 类型，表示 x 坐标

    public double Y => (double)Position[0]; // a // 将 Position 数组的第一个值转换为 double 类型，表示 y 坐标
}
```