# `.\better-genshin-impact\BetterGenshinImpact\View\Drawable\RectDrawable.cs`

```cs
﻿using BetterGenshinImpact.Core.Recognition.OpenCv; // 引入 OpenCvSharp 命名空间
using BetterGenshinImpact.GameTask; // 引入 GameTask 命名空间
using System; // 引入系统命名空间
using System.Drawing; // 引入绘图命名空间
using System.Windows; // 引入 Windows 窗口相关命名空间

namespace BetterGenshinImpact.View.Drawable // 定义命名空间
{
    [Serializable] // 表示该类可以被序列化
    public class RectDrawable
    {
        public string? Name { get; set; } // 定义可选的名称属性
        public Rect Rect { get; } // 定义矩形区域属性
        public Pen Pen { get; } = new(Color.Red, 2); // 定义画笔属性，默认为红色，宽度为2

        public RectDrawable(Rect rect, Pen? pen = null, string? name = null) // 构造函数，初始化矩形区域，画笔和名称
        {
            Rect = rect; // 设置矩形区域
            Name = name; // 设置名称

            if (pen != null) // 如果传入了画笔
            {
                Pen = pen; // 使用传入的画笔
            }
        }

        public RectDrawable(Rect rect, string? name) // 构造函数，初始化矩形区域和名称
        {
            Rect = rect; // 设置矩形区域
            Name = name; // 设置名称
        }

        public override bool Equals(object? obj) // 重写 Equals 方法，用于比较两个对象是否相等
        {
            if (obj == null || GetType() != obj.GetType()) // 如果对象为 null 或类型不匹配
            {
                return false; // 返回不相等
            }

            var other = (RectDrawable)obj; // 将对象转换为 RectDrawable 类型
            return Rect.Equals(other.Rect); // 比较矩形区域是否相等
        }

        public override int GetHashCode() // 重写 GetHashCode 方法，用于获取对象的哈希码
        {
            return Rect.GetHashCode(); // 返回矩形区域的哈希码
        }

        public bool IsEmpty => Rect.IsEmpty; // 判断矩形区域是否为空
    }

    /// <summary>
    /// 只有在坐标是整个游戏窗口捕获区域的时候才能使用这里的方法进行转换
    /// </summary>
    public static class RectDrawableExtension
    {
        public static RectDrawable ToRectDrawable(this Rect rect, Pen? pen = null, string? name = null) // 将 Rect 转换为 RectDrawable
        {
            var scale = TaskContext.Instance().DpiScale; // 获取当前 DPI 缩放比例
            Rect newRect = new(rect.X / scale, rect.Y / scale, rect.Width / scale, rect.Height / scale); // 根据 DPI 缩放比例调整矩形区域
            return new RectDrawable(newRect, pen, name); // 返回新的 RectDrawable 对象
        }

        public static RectDrawable ToRectDrawable(this OpenCvSharp.Rect rect, Pen? pen = null, string? name = null) // 将 OpenCvSharp.Rect 转换为 RectDrawable
        {
            var scale = TaskContext.Instance().DpiScale; // 获取当前 DPI 缩放比例
            OpenCvSharp.Rect newRect = new((int)(rect.X / scale), (int)(rect.Y / scale), (int)(rect.Width / scale), (int)(rect.Height / scale)); // 根据 DPI 缩放比例调整矩形区域
            return new RectDrawable(newRect.ToWindowsRectangle(), pen, name); // 返回新的 RectDrawable 对象
        }

        public static RectDrawable ToRectDrawable(this OpenCvSharp.Rect rect, int offsetX, int offsetY, Pen? pen = null, string? name = null) // 将 OpenCvSharp.Rect 转换为 RectDrawable，考虑偏移量
        {
            var scale = TaskContext.Instance().DpiScale; // 获取当前 DPI 缩放比例
            OpenCvSharp.Rect newRect = new(offsetX + (int)(rect.X / scale), offsetY + (int)(rect.Y / scale), (int)(rect.Width / scale), (int)(rect.Height / scale)); // 根据 DPI 缩放比例和偏移量调整矩形区域
            return new RectDrawable(newRect.ToWindowsRectangle(), pen, name); // 返回新的 RectDrawable 对象
        }
    }
}
```