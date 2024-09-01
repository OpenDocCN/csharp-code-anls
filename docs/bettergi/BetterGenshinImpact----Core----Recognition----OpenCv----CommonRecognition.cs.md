# `.\better-genshin-impact\BetterGenshinImpact\Core\Recognition\OpenCv\CommonRecognition.cs`

```cs
﻿using OpenCvSharp;
using System;
using System.Collections.Generic;
using System.Diagnostics;
using System.Linq;

namespace BetterGenshinImpact.Core.Recognition.OpenCv;

public class CommonRecognition
{
    /// <summary>
    ///     寻找游戏内按钮
    /// </summary>
    /// <param name="srcMat"></param>
    /// <returns></returns>
    public static List<Rect> FindGameButton(Mat srcMat)
    {
        try
        {
            // 克隆传入的图像矩阵，防止修改原始图像
            var src = srcMat.Clone();
            // 将图像从 BGR 颜色空间转换为 RGB 颜色空间
            Cv2.CvtColor(src, src, ColorConversionCodes.BGR2RGB);
            // 定义颜色范围，低端和高端都设置为相同的颜色
            var lowPurple = new Scalar(236, 229, 216);
            var highPurple = new Scalar(236, 229, 216);
            // 根据定义的颜色范围创建二值化图像
            Cv2.InRange(src, lowPurple, highPurple, src);
            // 对二值化图像进行阈值处理，将值大于 0 的像素设为 255（白色），其他设为 0（黑色）
            Cv2.Threshold(src, src, 0, 255, ThresholdTypes.Binary);
            // 定义结构元素进行膨胀操作（此行代码被注释掉了）
            //var kernel = Cv2.GetStructuringElement(MorphShapes.Rect, new OpenCvSharp.Size(20, 20),
            //    new OpenCvSharp.Point(-1, -1));
            // 使用结构元素对图像进行膨胀操作（此行代码被注释掉了）
            //Cv2.Dilate(src, src, kernel); //膨胀

            // 查找图像中的轮廓，返回轮廓列表
            Cv2.FindContours(src, out var contours, out _, RetrievalModes.External,
                ContourApproximationModes.ApproxSimple);
            // 如果找到轮廓
            if (contours.Length > 0)
            {
                // 计算每个轮廓的边界矩形，并过滤掉宽度小于 50 的矩形
                var boxes = contours.Select(Cv2.BoundingRect).Where(r => r.Width > 50);
                // 将结果转换为列表并返回
                return boxes.ToList();
            }
        }
        catch (Exception e)
        {
            // 捕获并输出异常信息
            Debug.WriteLine(e);
        }

        // 如果发生异常或未找到任何轮廓，返回空列表
        return [];
    }
}
```