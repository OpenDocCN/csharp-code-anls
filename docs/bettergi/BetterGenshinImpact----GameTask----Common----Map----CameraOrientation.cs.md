# `.\better-genshin-impact\BetterGenshinImpact\GameTask\Common\Map\CameraOrientation.cs`

```cs
﻿using BetterGenshinImpact.GameTask.Model.Area;
using OpenCvSharp;
using System;
using System.Collections.Generic;
using System.Drawing;
using System.Linq;
using Point = OpenCvSharp.Point;
using Size = OpenCvSharp.Size;

namespace BetterGenshinImpact.GameTask.Common.Map;

public class CameraOrientation
{
    /// <summary>
    /// 计算当前小地图摄像机朝向的角度
    /// </summary>
    /// <param name="greyMat">完整游戏截图</param>
    /// <returns>角度</returns>
    public static int Compute(Mat greyMat)
    {
        // 从完整游戏截图中提取小地图区域
        var mat = new Mat(greyMat, new Rect(62, 19, 212, 212));
        // 计算小地图上的摄像机朝向角度
        return ComputeMiniMap(mat);
    }

    /// <summary>
    /// 计算当前小地图摄像机朝向的角度
    /// </summary>
    /// <param name="mat">小地图灰度图</param>
    /// <returns>角度</returns>
    public static int ComputeMiniMap(Mat mat)
    {
        // 对小地图进行高斯模糊
        Cv2.GaussianBlur(mat, mat, new Size(3, 3), 0);
        // 设置极坐标转换的中心点
        var centerPoint = new Point2f(mat.Width / 2f, mat.Height / 2f);
        // 创建极坐标图像
        var polarMat = new Mat();
        Cv2.WarpPolar(mat, polarMat, new Size(360, 360), centerPoint, 360d, InterpolationFlags.Linear, WarpPolarMode.Linear);
        // Cv2.ImShow("polarMat", polarMat);  // 可视化极坐标图像
        // 从极坐标图像中提取感兴趣区域，并旋转90度
        var polarRoiMat = new Mat(polarMat, new Rect(10, 0, 70, polarMat.Height));
        Cv2.Rotate(polarRoiMat, polarRoiMat, RotateFlags.Rotate90Counterclockwise);

        // 计算 Scharr 边缘检测结果
        var scharrResult = new Mat();
        Cv2.Scharr(polarRoiMat, scharrResult, MatType.CV_32F, 1, 0);

        // 初始化左右波峰计数数组
        var left = new int[360];
        var right = new int[360];

        // 提取 Scharr 结果的数组形式
        scharrResult.GetArray<float>(out var array);
        // 查找左侧波峰
        var leftPeaks = FindPeaks(array);
        leftPeaks.ForEach(i => left[i % 360]++);

        // 反转数组并查找右侧波峰
        var reversedArray = array.Select(x => -x).ToArray();
        var rightPeaks = FindPeaks(reversedArray);
        rightPeaks.ForEach(i => right[i % 360]++);

        // 优化左右波峰数据
        var left2 = left.Zip(right, (x, y) => Math.Max(x - y, 0)).ToArray();
        var right2 = right.Zip(left, (x, y) => Math.Max(x - y, 0)).ToArray();

        // 计算左移后相乘的和，在附近2°内寻找最大值
        var sum = new int[360];
        for (var i = -2; i <= 2; i++)
        {
            var all = left2.Zip(Shift(right2, -90 + i), (x, y) => x * y * (3 - Math.Abs(i)) / 3).ToArray();
            sum = sum.Zip(all, (x, y) => x + y).ToArray();
        }

        // 对结果进行卷积
        var result = new int[360];
        for (var i = -2; i <= 2; i++)
        {
            var all = Shift(sum, i);
            for (var j = 0; j < all.Length; j++)
            {
                all[j] = all[j] * (3 - Math.Abs(i)) / 3;
            }

            result = result.Zip(all, (x, y) => x + y).ToArray();
        }

        // 计算角度的最大值及其索引
        var maxIndex = result.ToList().IndexOf(result.Max());
        var angle = maxIndex + 45;
        if (angle > 360)
        {
            angle -= 360;
        }

        return angle;
    }

    public static void DrawDirection(ImageRegion region, double angle, string name = "camera", Pen? pen = null)
    {
        // 绘图
        // 获取系统信息中的资产比例
        var scale = TaskContext.Instance().SystemInfo.AssetScale;
        // 定义半径
        const int r = 100;
        // 计算地图中心点的坐标
        var center = new Point(168 * scale, 125 * scale); // 地图中心点 后续建议调整
        // 计算圆弧上点的 x 坐标
        var x1 = center.X + r * Math.Cos(angle * Math.PI / 180);
        // 计算圆弧上点的 y 坐标
        var y1 = center.Y + r * Math.Sin(angle * Math.PI / 180);

        // 创建绘制的线对象并设置其属性（被注释掉）
        // var line = new LineDrawable(center, new Point(x1, y1))
        // {
        //     Pen = new Pen(Color.Yellow, 1)
        // };
        // 使用 VisionContext 绘制线条（被注释掉）
        // VisionContext.Instance().DrawContent.PutLine("camera", line);

        // 如果 pen 为 null，则创建一个新的 Pen 对象
        pen ??= new Pen(Color.Yellow, 1);

        // 在区域中绘制从 center 到 (x1, y1) 的线段
        region.DrawLine(center.X, center.Y, (int)x1, (int)y1, name, pen);
    }

    // 查找数据中的峰值
    static List<int> FindPeaks(float[] data)
    {
        // 初始化峰值索引的列表
        List<int> peakIndices = [];

        // 遍历数据中的每个元素（排除第一个和最后一个元素）
        for (int i = 1; i < data.Length - 1; i++)
        {
            // 如果当前元素大于前一个和后一个元素，视为峰值
            if (data[i] > data[i - 1] && data[i] > data[i + 1])
            {
                // 添加峰值的索引到列表中
                peakIndices.Add(i);
            }
        }

        // 返回峰值索引列表
        return peakIndices;
    }

    // 右移数组中的元素
    public static int[] RightShift(int[] array, int k)
    {
        // 返回右移后的新数组
        return array.Skip(array.Length - k)
            .Concat(array.Take(array.Length - k))
            .ToArray();
    }

    // 左移数组中的元素
    public static int[] LeftShift(int[] array, int k)
    {
        // 返回左移后的新数组
        return array.Skip(k)
            .Concat(array.Take(k))
            .ToArray();
    }

    // 根据 k 的符号决定是右移还是左移
    public static int[] Shift(int[] array, int k)
    {
        // 如果 k 为正数，进行右移
        if (k > 0)
        {
            return RightShift(array, k);
        }
        else
        {
            // 如果 k 为负数，进行左移
            return LeftShift(array, -k);
        }
    }
# 代码块的结束标志
}
```