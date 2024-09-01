# `.\better-genshin-impact\BetterGenshinImpact\View\Drawable\LineDrawable.cs`

```cs
﻿using System;  // 引入 System 命名空间
using System.Drawing;  // 引入 System.Drawing 命名空间
using Point = System.Windows.Point;  // 使用 System.Windows.Point 作为 Point 的别名

namespace BetterGenshinImpact.View.Drawable;  // 定义 BetterGenshinImpact.View.Drawable 命名空间

[Serializable]  // 表示该类可以被序列化
public class LineDrawable
{
    public Point P1 { get; set; }  // 起点属性，类型为 Point

    public Point P2 { get; set; }  // 终点属性，类型为 Point

    public Pen Pen { get; set; } = new(Color.Red, 2);  // Pen 属性，默认初始化为红色，线宽为 2

    public LineDrawable(double x1, double y1, double x2, double y2)
    {
        P1 = new Point(x1, y1);  // 使用指定的坐标初始化起点
        P2 = new Point(x2, y2);  // 使用指定的坐标初始化终点
    }

    public LineDrawable(Point p1, Point p2)
    {
        P1 = p1;  // 使用指定的 Point 对象初始化起点
        P2 = p2;  // 使用指定的 Point 对象初始化终点
    }

    // public LineDrawable(OpenCvSharp.Point p1, OpenCvSharp.Point p2)
    // {
    //     var scale = TaskContext.Instance().DpiScale;  // 获取 DPI 缩放比例
    //     P1 = Divide(p1, scale).ToWindowsPoint();  // 将 OpenCvSharp.Point 转换为 Windows.Point
    //     P2 = Divide(p2, scale).ToWindowsPoint();  // 将 OpenCvSharp.Point 转换为 Windows.Point
    // }
    //
    // public static OpenCvSharp.Point Divide(OpenCvSharp.Point p, float divisor)
    // {
    //     return new OpenCvSharp.Point(p.X / divisor, p.Y / divisor);  // 将 OpenCvSharp.Point 按比例缩放
    // }

    public override bool Equals(object? obj)
    {
        if (obj == null || GetType() != obj.GetType())
        {
            return false;  // 如果对象为空或类型不同，返回 false
        }

        var other = (LineDrawable)obj;  // 将对象转换为 LineDrawable 类型
        return P1.Equals(other.P1) && P2.Equals(other.P2);  // 比较起点和终点是否相等
    }

    public override int GetHashCode()
    {
        return P1.GetHashCode() + P2.GetHashCode();  // 计算哈希码，基于起点和终点的哈希码
    }
}
```