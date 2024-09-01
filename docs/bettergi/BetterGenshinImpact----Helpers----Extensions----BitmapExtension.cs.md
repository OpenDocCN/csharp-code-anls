# `.\better-genshin-impact\BetterGenshinImpact\Helpers\Extensions\BitmapExtension.cs`

```cs
﻿using OpenCvSharp;  // 导入 OpenCvSharp 库
using System.Drawing;  // 导入 System.Drawing 命名空间，用于处理图形
using System.IO;  // 导入 System.IO 命名空间，用于处理流
using System.Windows.Media.Imaging;  // 导入 System.Windows.Media.Imaging 命名空间，用于处理 WPF 图像

namespace BetterGenshinImpact.Helpers.Extensions  // 定义命名空间
{
    public static class BitmapExtension  // 定义静态类 BitmapExtension
    {
        // 将 Bitmap 对象转换为 BitmapImage 对象的扩展方法
        public static BitmapImage ToBitmapImage(this Bitmap bitmap)
        {
            var ms = new MemoryStream();  // 创建一个内存流
            bitmap.Save(ms, System.Drawing.Imaging.ImageFormat.Bmp);  // 将 Bitmap 保存到内存流中，格式为 BMP
            var image = new BitmapImage();  // 创建一个 BitmapImage 对象
            image.BeginInit();  // 初始化 BitmapImage 对象
            ms.Seek(0, SeekOrigin.Begin);  // 将内存流的位置重新定位到开始
            image.StreamSource = ms;  // 设置 BitmapImage 的数据源为内存流
            image.EndInit();  // 结束 BitmapImage 对象的初始化
            return image;  // 返回转换后的 BitmapImage 对象
        }

        // 将 Color 对象转换为 Scalar 对象的扩展方法
        public static Scalar ToScalar(this Color color)
        {
            return new Scalar(color.R, color.G, color.B);  // 创建一个 Scalar 对象，使用颜色的 R、G 和 B 分量
        }
    }
}
```