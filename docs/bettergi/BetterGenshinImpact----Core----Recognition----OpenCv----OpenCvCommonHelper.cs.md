# `.\better-genshin-impact\BetterGenshinImpact\Core\Recognition\OpenCv\OpenCvCommonHelper.cs`

```cs
﻿using OpenCvSharp;
using System.Diagnostics;

namespace BetterGenshinImpact.Core.Recognition.OpenCv;

public class OpenCvCommonHelper
{
    /// <summary>
    ///     计算灰度图中某个颜色的像素个数
    ///     快速遍历方法来自于: https://blog.csdn.net/TyroneKing/article/details/129108838
    /// </summary>
    /// <param name="mat"></param>
    /// <param name="color"></param>
    /// <returns></returns>
    public static int CountGrayMatColor(Mat mat, byte color)
    {
        // 确保输入的 Mat 对象是 8 位单通道图像
        Debug.Assert(mat.Depth() == MatType.CV_8U);
        // 获取图像的通道数
        var channels = mat.Channels();
        // 获取图像的行数
        var nRows = mat.Rows;
        // 计算每行的列数（通道数乘以列数）
        var nCols = mat.Cols * channels;
        // 检查图像是否是连续的
        if (mat.IsContinuous())
        {
            // 如果是连续的，则每列的数量乘以行数，并将行数设为1
            nCols *= nRows;
            nRows = 1;
        }

        // 统计颜色匹配的像素数量
        var sum = 0;
        unsafe
        {
            // 遍历每一行
            for (var i = 0; i < nRows; i++)
            {
                // 获取当前行的指针
                var p = mat.Ptr(i);
                // 将指针转换为字节指针
                var b = (byte*)p.ToPointer();
                // 遍历每一列
                for (var j = 0; j < nCols; j++)
                    // 如果当前像素的值等于指定颜色，则计数加一
                    if (b[j] == color)
                        sum++;
            }
        }

        // 返回匹配颜色的像素数量
        return sum;
    }

    public static int CountGrayMatColor(Mat mat, byte lowColor, byte highColor)
    {
        // 确保输入的 Mat 对象是 8 位单通道图像
        Debug.Assert(mat.Depth() == MatType.CV_8U);
        // 获取图像的通道数
        var channels = mat.Channels();
        // 获取图像的行数
        var nRows = mat.Rows;
        // 计算每行的列数（通道数乘以列数）
        var nCols = mat.Cols * channels;
        // 检查图像是否是连续的
        if (mat.IsContinuous())
        {
            // 如果是连续的，则每列的数量乘以行数，并将行数设为1
            nCols *= nRows;
            nRows = 1;
        }

        // 统计颜色范围内的像素数量
        var sum = 0;
        unsafe
        {
            // 遍历每一行
            for (var i = 0; i < nRows; i++)
            {
                // 获取当前行的指针
                var p = mat.Ptr(i);
                // 将指针转换为字节指针
                var b = (byte*)p.ToPointer();
                // 遍历每一列
                for (var j = 0; j < nCols; j++)
                    // 如果当前像素的值在指定的颜色范围内，则计数加一
                    if (b[j] >= lowColor && b[j] <= highColor)
                        sum++;
            }
        }

        // 返回在颜色范围内的像素数量
        return sum;
    }

    public static Mat Threshold(Mat src, Scalar low, Scalar high)
    {
        // 使用临时变量存储结果
        using var mask = new Mat();
        using var rgbMat = new Mat();

        // 将源图像从 BGR 转换为 RGB
        Cv2.CvtColor(src, rgbMat, ColorConversionCodes.BGR2RGB);
        // 在 RGB 图像中进行颜色范围过滤
        Cv2.InRange(rgbMat, low, high, mask);
        // 对过滤后的图像进行二值化处理
        Cv2.Threshold(mask, mask, 0, 255, ThresholdTypes.Binary); //二值化
        // 返回处理后的图像的副本
        return mask.Clone();
    }

    public static Mat InRangeHsv(Mat src, Scalar low, Scalar high)
    {
        // 使用临时变量存储结果
        using var mask = new Mat();
        using var rgbMat = new Mat();

        // 将源图像从 BGR 转换为 HSV
        Cv2.CvtColor(src, rgbMat, ColorConversionCodes.BGR2HSV);
        // 在 HSV 图像中进行颜色范围过滤
        Cv2.InRange(rgbMat, low, high, mask);
        // 返回处理后的图像的副本
        return mask.Clone();
    }

    public static Mat Threshold(Mat src, Scalar s)
    {
        // 调用阈值处理函数，范围参数设置为相同的值
        return Threshold(src, s, s);
    }

    /// <summary>
    ///     和二值化的颜色刚好相反
    /// </summary>
    /// <param name="src"></param>
    /// <param name="s"></param>
    /// <returns></returns>
    public static Mat CreateMask(Mat src, Scalar s)
    {
        // 创建一个用于存储掩码的 Mat 对象
        var mask = new Mat();
        // 在源图像中进行颜色范围过滤
        Cv2.InRange(src, s, s, mask);
        // 返回掩码图像的反转
        return 255 - mask;
    }
}
```