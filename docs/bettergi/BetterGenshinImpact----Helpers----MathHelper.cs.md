# `.\better-genshin-impact\BetterGenshinImpact\Helpers\MathHelper.cs`

```cs
﻿using OpenCvSharp;  // 引入 OpenCvSharp 库，用于处理图像和计算几何
using System;        // 引入 System 命名空间，包含基本的系统功能

namespace BetterGenshinImpact.Helpers  // 定义命名空间 BetterGenshinImpact.Helpers
{
    // 定义一个静态类 MathHelper，包含数学计算相关的方法
    public class MathHelper
    {
        /// <summary>
        /// 点到直线的最短距离
        /// </summary>
        /// <param name="point"></param>  // 输入参数，待计算的点
        /// <param name="point1"></param> // 输入参数，直线上的第一个点
        /// <param name="point2"></param> // 输入参数，直线上的第二个点
        /// <returns></returns>         // 返回值，点到直线的最短距离
        public static double Distance(Point point, Point point1, Point point2)
        {
            // 计算直线的方向向量的系数 a、b、c
            double a = point2.Y - point1.Y;  // 直线方向向量的 y 方向分量
            double b = point1.X - point2.X;  // 直线方向向量的 x 方向分量
            double c = point2.X * point1.Y - point1.X * point2.Y;  // 直线方程的常数项

            // 使用直线距离公式计算点到直线的最短距离
            double numerator = Math.Abs(a * point.X + b * point.Y + c);  // 直线方程的绝对值
            double denominator = Math.Sqrt(a * a + b * b);  // 直线方向向量的长度
            double distance = numerator / denominator;  // 计算点到直线的距离

            return distance;  // 返回计算出的距离
        }

        /// <summary>
        /// 两点之间的距离
        /// </summary>
        /// <param name="p1"></param>  // 输入参数，第一个点
        /// <param name="p2"></param>  // 输入参数，第二个点
        /// <returns></returns>      // 返回值，两点之间的距离
        public static double Distance(Point p1, Point p2)
        {
            // 使用欧几里得距离公式计算两点之间的距离
            return Math.Sqrt(Math.Pow(p1.X - p2.X, 2) + Math.Pow(p1.Y - p2.Y, 2));
        }

        // 重载方法，用于处理浮点数类型的点
        public static double Distance(Point2f p1, Point2f p2)
        {
            // 使用欧几里得距离公式计算两点之间的距离
            return Math.Sqrt(Math.Pow(p1.X - p2.X, 2) + Math.Pow(p1.Y - p2.Y, 2));
        }
    }
}
```