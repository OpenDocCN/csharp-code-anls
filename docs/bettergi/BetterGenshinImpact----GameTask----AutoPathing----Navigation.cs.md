# `.\better-genshin-impact\BetterGenshinImpact\GameTask\AutoPathing\Navigation.cs`

```cs
﻿using BetterGenshinImpact.GameTask.AutoPathing.Model; // 引用 BetterGenshinImpact 项目中的 AutoPathing.Model 命名空间
using BetterGenshinImpact.GameTask.Common.Map; // 引用 BetterGenshinImpact 项目中的 Common.Map 命名空间
using BetterGenshinImpact.GameTask.Common; // 引用 BetterGenshinImpact 项目中的 Common 命名空间
using OpenCvSharp; // 引用 OpenCVSharp 库，用于图像处理
using System; // 引用系统基础类库
using System.Collections.Generic; // 引用系统集合类库
using System.Text; // 引用系统文本处理类库
using BetterGenshinImpact.GameTask.Model.Area; // 引用 BetterGenshinImpact 项目中的 Model.Area 命名空间

namespace BetterGenshinImpact.GameTask.AutoPathing; // 定义命名空间 BetterGenshinImpact.GameTask.AutoPathing
internal class Navigation // 定义内部类 Navigation
{

    internal static Point2f GetPosition(ImageRegion imageRegion) // 定义内部静态方法 GetPosition，接受一个 ImageRegion 类型参数
    {
        // 从 imageRegion 的 SrcGreyMat 中提取一个 212x212 的矩形区域，创建一个新的 Mat 对象
        var greyMat = new Mat(imageRegion.SrcGreyMat, new Rect(62, 19, 212, 212));
        // 使用 EntireMap 实例的 GetMiniMapPositionByFeatureMatch 方法，通过特征匹配获取迷你地图上的位置
        return EntireMap.Instance.GetMiniMapPositionByFeatureMatch(greyMat);
    }

    internal static int GetTargetOrientation(Waypoint waypoint, Point2f position) // 定义内部静态方法 GetTargetOrientation，接受一个 Waypoint 类型和一个 Point2f 类型的参数
    {
        // 计算目标点和当前位置在 x 轴和 y 轴上的距离差
        double deltaX = waypoint.X - position.X;
        double deltaY = waypoint.Y - position.Y;
        // 计算两点之间的距离
        double vectorLength = Math.Sqrt(deltaX * deltaX + deltaY * deltaY);
        // 如果距离为零，则返回 0，表示角度无法定义
        if (vectorLength == 0)
        {
            return 0;
        }
        // 计算向量与 x 轴之间的夹角（逆时针方向）
        double angle = Math.Acos(deltaX / vectorLength);
        // 如果 deltaY 小于 0，说明向量在 x 轴下方，需要调整角度
        if (deltaY < 0)
        {
            angle = 2 * Math.PI - angle;
        }
        // 将角度从弧度转换为度，并返回
        return (int)(angle * (180.0 / Math.PI));
    }

    internal static double GetDistance(Waypoint waypoint, Point2f position) // 定义内部静态方法 GetDistance，接受一个 Waypoint 类型和一个 Point2f 类型的参数
    {
        // 获取 Waypoint 的 x 和 y 坐标
        var x1 = waypoint.X;
        var y1 = waypoint.Y;
        // 获取当前位置的 x 和 y 坐标
        var x2 = position.X;
        var y2 = position.Y;
        // 计算两点之间的欧几里得距离
        return Math.Sqrt((x2 - x1) * (x2 - x1) + (y2 - y1) * (y2 - y1));
    }
}
```