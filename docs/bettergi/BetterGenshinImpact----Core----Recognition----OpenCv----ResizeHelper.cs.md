# `.\better-genshin-impact\BetterGenshinImpact\Core\Recognition\OpenCv\ResizeHelper.cs`

```cs
﻿using OpenCvSharp; // 引入 OpenCvSharp 库以使用 OpenCV 函数
using System; // 引入 System 命名空间以使用基础功能

namespace BetterGenshinImpact.Core.Recognition.OpenCv; // 定义命名空间

public class ResizeHelper // 定义 ResizeHelper 类
{
    /// <summary>
    ///     等比放大的 // 文档注释，描述该方法功能
    /// </summary>
    /// <param name="src"></param> // 输入图像
    /// <param name="scale"></param> // 缩放比例
    /// <param name="interpolation"></param> // 插值方式
    /// <returns></returns> // 返回缩放后的图像
    public static Mat Resize(Mat src, double scale, InterpolationFlags interpolation = InterpolationFlags.Linear)
    {
        if (Math.Abs(scale - 1) > 0.00001) // 判断缩放比例是否接近1
        {
            // 调用另一个重载的 Resize 方法，以宽高均按比例缩放
            return Resize(src, scale, scale, interpolation);
        }
        return src; // 如果比例接近1，返回原图像
    }

    public static Mat Resize(Mat src, double widthScale, double heightScale, InterpolationFlags interpolation = InterpolationFlags.Linear)
    {
        if (Math.Abs(widthScale - 1) > 0.00001 || Math.Abs(heightScale - 1) > 0.00001) // 判断宽高缩放比例是否接近1
        {
            var dst = new Mat(); // 创建一个新的 Mat 对象用于存储缩放后的图像
            // 使用 Cv2.Resize 函数按给定宽高比例缩放图像
            Cv2.Resize(src, dst, new Size(src.Width * widthScale, src.Height * heightScale), 0, 0, interpolation);
            return dst; // 返回缩放后的图像
        }

        return src; // 如果宽高比例接近1，返回原图像
    }

    public static Mat ResizeTo(Mat src, int width, int height)
    {
        if (src.Width != width || src.Height != height) // 判断目标宽高是否与原图像不同
        {
            var dst = new Mat(); // 创建一个新的 Mat 对象用于存储缩放后的图像
            // 使用 Cv2.Resize 函数将图像调整为指定的宽高
            Cv2.Resize(src, dst, new Size(width, height));
            return dst; // 返回调整后的图像
        }

        return src; // 如果目标宽高与原图像相同，返回原图像
    }
}
```