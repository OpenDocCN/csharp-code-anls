# `.\better-genshin-impact\BetterGenshinImpact\Core\Recognition\OpenCv\CommonExtension.cs`

```cs
# 引入 OpenCvSharp 库以及其他需要的命名空间
﻿using OpenCvSharp;
using System;
using System.Collections.Generic;
using System.Drawing;
using Vanara.PInvoke;
using Color = System.Windows.Media.Color;
using Point = OpenCvSharp.Point;

namespace BetterGenshinImpact.Core.Recognition.OpenCv;

# 定义一个公共静态类，包含各种扩展方法
public static class CommonExtension
{
    # 将 System.Drawing.Point 转换为 OpenCvSharp.Point
    public static unsafe Point AsCvPoint(this System.Drawing.Point point)
    {
        return *(Point*)&point;
    }

    # 将 OpenCvSharp.Point 转换为 System.Drawing.Point
    public static unsafe System.Drawing.Point AsDrawingPoint(this Point point)
    {
        return *(System.Drawing.Point*)&point;
    }

    # 将 OpenCvSharp.Point 转换为 System.Windows.Point
    public static System.Windows.Point ToWindowsPoint(this Point point)
    {
        return new System.Windows.Point(point.X, point.Y);
    }

    # 将 Rectangle 转换为 OpenCvSharp.Rect
    public static unsafe Rect AsCvRect(this Rectangle rectangle)
    {
        return *(Rect*)&rectangle;
    }

    # 将 OpenCvSharp.Rect 转换为 System.Windows.Rect
    public static System.Windows.Rect ToWindowsRectangle(this Rect rect)
    {
        return new System.Windows.Rect(rect.X, rect.Y, rect.Width, rect.Height);
    }

    # 将 OpenCvSharp.Rect 转换为 System.Windows.Rect，并应用偏移量
    public static System.Windows.Rect ToWindowsRectangleOffset(this Rect rect, int offsetX, int offsetY)
    {
        return new System.Windows.Rect(rect.X + offsetX, rect.Y + offsetY, rect.Width, rect.Height);
    }

    # 将 OpenCvSharp.Rect 转换为 System.Drawing.Rectangle
    public static unsafe Rectangle AsDrawingRectangle(this Rect rect)
    {
        return *(Rectangle*)&rect;
    }

    # 获取 System.Drawing.Rectangle 的中心点
    public static System.Drawing.Point GetCenterPoint(this Rectangle rectangle)
    {
        if (rectangle.IsEmpty) throw new ArgumentException("rectangle is empty");

        return new System.Drawing.Point(rectangle.X + rectangle.Width / 2, rectangle.Y + rectangle.Height / 2);
    }

    # 获取 RECT 的中心点
    public static Point GetCenterPoint(this RECT rectangle)
    {
        if (rectangle.IsEmpty) throw new ArgumentException("rectangle is empty");

        return new Point(rectangle.X + rectangle.Width / 2, rectangle.Y + rectangle.Height / 2);
    }

    # 获取 OpenCvSharp.Rect 的中心点
    public static Point GetCenterPoint(this Rect rectangle)
    {
        if (rectangle == Rect.Empty) throw new ArgumentException("rectangle is empty");

        return new Point(rectangle.X + rectangle.Width / 2, rectangle.Y + rectangle.Height / 2);
    }

    # 将 OpenCvSharp.Rect 进行缩放
    public static Rect Multiply(this Rect rect, double assetScale)
    {
        if (rect == Rect.Empty) throw new ArgumentException("rect is empty");

        return new Rect((int)(rect.X * assetScale), (int)(rect.Y * assetScale), (int)(rect.Width * assetScale), (int)(rect.Height * assetScale));
    }

    # 将 System.Drawing.Color 转换为 System.Windows.Media.Color
    public static Color ToWindowsColor(this System.Drawing.Color color)
    {
        return Color.FromArgb(color.A, color.R, color.G, color.B);
    }

    # 将 OpenCvSharp.Point2f 转换为 OpenCvSharp.Point2d
    public static Point2d ToPoint2d(this Point2f p)
    {
        return new Point2d(p.X, p.Y);
    }

    # 将 List<Point2f> 转换为 List<Point2d>
    public static List<Point2d> ToPoint2d(this List<Point2f> list)
    {
        return list.ConvertAll(ToPoint2d);
    }
}
```