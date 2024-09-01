# `.\better-genshin-impact\BetterGenshinImpact\GameTask\AutoTrackPath\Model\GiPath.cs`

```cs
﻿using BetterGenshinImpact.Helpers; // 引入 BetterGenshinImpact.Helpers 命名空间
using OpenCvSharp; // 引入 OpenCvSharp 命名空间
using System; // 引入 System 命名空间
using System.Collections.Generic; // 引入 System.Collections.Generic 命名空间
using System.Diagnostics; // 引入 System.Diagnostics 命名空间

namespace BetterGenshinImpact.GameTask.AutoTrackPath.Model; // 定义 BetterGenshinImpact.GameTask.AutoTrackPath.Model 命名空间

[Serializable] // 指定 GiPath 类是可序列化的
public class GiPath
{
    // 初始化 WayPointList 属性为空的 List<GiPathPoint> 实例
    public List<GiPathPoint> WayPointList { get; set; } = [];

    // 添加新的路径点到 WayPointList
    public void AddPoint(Point2f point)
    {
        // 创建 GiPathPoint 实例并初始化
        var giPathPoint = GiPathPoint.BuildFrom(point, WayPointList.Count);
        // 如果 WayPointList 中已经有点
        if (WayPointList.Count > 0)
        {
            // 获取 WayPointList 中的最后一个点
            var lastPoint = WayPointList[^1];
            // 计算当前点与最后一个点之间的距离
            var distance = MathHelper.Distance(giPathPoint.Pt, lastPoint.Pt);
            // 如果距离为 0 或者大于 50
            if (distance == 0 || distance > 50)
            {
                // 输出调试信息并终止添加操作
                Debug.WriteLine($"距离上个点太近或者太远: {distance}，舍弃");
                return;
            }
        }

        // 将 giPathPoint 添加到 WayPointList 中
        WayPointList.Add(giPathPoint);
    }
}
```