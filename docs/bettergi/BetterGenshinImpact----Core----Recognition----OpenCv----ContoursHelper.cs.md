# `.\better-genshin-impact\BetterGenshinImpact\Core\Recognition\OpenCv\ContoursHelper.cs`

```cs
﻿using OpenCvSharp; // 导入 OpenCvSharp 库
using System; // 导入 System 库
using System.Collections.Generic; // 导入集合库
using System.Diagnostics; // 导入调试库
using System.Linq; // 导入 LINQ 库

namespace BetterGenshinImpact.Core.Recognition.OpenCv; // 定义命名空间

public class ContoursHelper
{
    /// <summary>
    /// 找到指定颜色的矩形
    /// </summary>
    /// <param name="srcMat">图像</param>
    /// <param name="low">RGB色彩低位</param>
    /// <param name="high">RGB色彩高位</param>
    /// <param name="minWidth">矩形最小宽度</param>
    /// <param name="minHeight">矩形最小高度</param>
    /// <returns></returns>
    public static List<Rect> FindSpecifyColorRects(Mat srcMat, Scalar low, Scalar high, int minWidth = -1, int minHeight = -1)
    {
        try
        {
            // 克隆原始图像
            using var src = srcMat.Clone();
            // 将图像从 BGR 转换为 RGB
            Cv2.CvtColor(src, src, ColorConversionCodes.BGR2RGB);
            // 根据指定的颜色范围创建掩码
            Cv2.InRange(src, low, high, src);

            // 查找图像中的轮廓
            Cv2.FindContours(src, out var contours, out _, RetrievalModes.External,
                ContourApproximationModes.ApproxSimple);
            if (contours.Length > 0)
            {
                // 对每个轮廓计算其边界矩形，过滤掉不满足尺寸要求的矩形
                var boxes = contours.Select(Cv2.BoundingRect).Where(r =>
                {
                    if (minWidth > 0 && r.Width < minWidth)
                    {
                        return false;
                    }
                    if (minHeight > 0 && r.Height < minHeight)
                    {
                        return false;
                    }
                    return true;
                });
                // 返回符合条件的矩形列表
                return boxes.ToList();
            }
        }
        catch (Exception e)
        {
            // 捕捉异常并输出到调试窗口
            Debug.WriteLine(e);
        }

        // 返回空列表，表示没有找到符合条件的矩形
        return [];
    }

    // 重载方法，使用相同的颜色作为低位和高位
    public static List<Rect> FindSpecifyColorRects(Mat srcMat, Scalar color, int minWidth = -1, int minHeight = -1)
    {
        return FindSpecifyColorRects(srcMat, color, color, minWidth, minHeight);
    }
}
```