# `.\better-genshin-impact\BetterGenshinImpact\Core\Recognition\OpenCv\ArithmeticHelper.cs`

```cs
﻿using OpenCvSharp;

namespace BetterGenshinImpact.Core.Recognition.OpenCv;

public class ArithmeticHelper
{
    /// <summary>
    ///     水平投影
    /// </summary>
    /// <param name="gray"></param>
    /// <returns></returns>
    public static int[] HorizontalProjection(Mat gray)
    {
        // 创建一个数组来存储每一行的投影值，数组长度等于图像的高度
        var projection = new int[gray.Height];
        // 对每一行计算投影值
        for (var y = 0; y < gray.Height; ++y)
            // 遍历这一行的每一个像素，如果是有效的，累加投影值
            for (var x = 0; x < gray.Width; ++x)
            {
                // 获取指定位置的像素值
                var s = gray.Get<Vec2b>(y, x);
                // 如果像素的第一个值为 255（白色），则累加到当前行的投影值中
                if (s.Item0 == 255) projection[y]++;
            }

        // 返回水平投影结果
        return projection;
    }

    /// <summary>
    ///     垂直投影
    /// </summary>
    /// <param name="gray"></param>
    /// <returns></returns>
    public static int[] VerticalProjection(Mat gray)
    {
        // 创建一个数组来存储每一列的投影值，数组长度等于图像的宽度
        var projection = new int[gray.Width];
        // 遍历每一列计算投影值
        for (var x = 0; x < gray.Width; ++x)
            // 遍历这一列的每一个像素，如果是有效的，累加投影值
            for (var y = 0; y < gray.Height; ++y)
            {
                // 获取指定位置的像素值
                var s = gray.Get<Vec2b>(y, x);
                // 如果像素的第一个值为 255（白色），则累加到当前列的投影值中
                if (s.Item0 == 255) projection[x]++;
            }

        // 返回垂直投影结果
        return projection;
    }
}
```