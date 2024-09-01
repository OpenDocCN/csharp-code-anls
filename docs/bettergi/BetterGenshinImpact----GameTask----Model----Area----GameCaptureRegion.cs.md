# `.\better-genshin-impact\BetterGenshinImpact\GameTask\Model\Area\GameCaptureRegion.cs`

```cs
﻿using BetterGenshinImpact.GameTask.Model.Area.Converter;  // 引入用于区域转换的命名空间
using BetterGenshinImpact.View.Drawable;  // 引入用于可绘制对象的命名空间
using OpenCvSharp;  // 引入 OpenCV 库用于图像处理
using System;  // 引入系统基础功能的命名空间
using System.Drawing;  // 引入绘图功能的命名空间
using Size = OpenCvSharp.Size;  // 将 OpenCvSharp 的 Size 类型命名为 Size，以避免与 System.Drawing.Size 冲突

namespace BetterGenshinImpact.GameTask.Model.Area  // 定义命名空间
{
    /// <summary>
    /// 游戏捕获区域类
    /// 主要用于转换到遮罩窗口的坐标
    /// </summary>
    public class GameCaptureRegion(Bitmap bitmap, int initX, int initY, Region? owner = null, INodeConverter? converter = null) : ImageRegion(bitmap, initX, initY, owner, converter)
    {
        /// <summary>
        /// 在游戏捕获图像的坐标维度进行转换到遮罩窗口的坐标维度
        /// </summary>
        /// <param name="x"></param>
        /// <param name="y"></param>
        /// <param name="w"></param>
        /// <param name="h"></param>
        /// <param name="pen"></param>
        /// <param name="name"></param>
        /// <returns></returns>
        public RectDrawable ConvertToRectDrawable(int x, int y, int w, int h, Pen? pen = null, string? name = null)
        {
            // 获取 DPI 缩放比例
            var scale = TaskContext.Instance().DpiScale;
            // 根据缩放比例计算新的矩形区域
            System.Windows.Rect newRect = new(x / scale, y / scale, w / scale, h / scale);
            // 返回一个新的 RectDrawable 对象
            return new RectDrawable(newRect, pen, name);
        }

        /// <summary>
        /// 在游戏捕获图像的坐标维度进行转换到遮罩窗口的坐标维度
        /// </summary>
        /// <param name="x1"></param>
        /// <param name="y1"></param>
        /// <param name="x2"></param>
        /// <param name="y2"></param>
        /// <param name="pen"></param>
        /// <param name="name"></param>
        /// <returns></returns>
        public LineDrawable ConvertToLineDrawable(int x1, int y1, int x2, int y2, Pen? pen = null, string? name = null)
        {
            // 获取 DPI 缩放比例
            var scale = TaskContext.Instance().DpiScale;
            // 根据缩放比例计算新的 LineDrawable 对象
            var drawable = new LineDrawable(x1 / scale, y1 / scale, x2 / scale, y2 / scale);
            // 如果提供了笔刷，设置到 LineDrawable 对象中
            if (pen != null)
            {
                drawable.Pen = pen;
            }
            // 返回 LineDrawable 对象
            return drawable;
        }

        // public void DrawRect(int x, int y, int w, int h, Pen? pen = null, string? name = null)
        // {
        //     VisionContext.Instance().DrawContent.PutRect(name ?? "None", ConvertToRectDrawable(x, y, w, h, pen, name));
        // }

        /// <summary>
        /// 游戏窗口初始截图大于1080P的统一转换到1080P
        /// </summary>
        /// <returns></returns>
        public ImageRegion DeriveTo1080P()
        {
            // 如果宽度不大于1920，返回当前对象
            if (Width <= 1920)
            {
                return this;
            }
            // 计算缩放比例
            var scale = Width / 1920d;

            // 创建新的 Mat 对象用于存储调整后的图像
            var newMat = new Mat();
            // 使用 OpenCV 调整图像大小到 1920 宽度，保持纵横比
            Cv2.Resize(SrcMat, newMat, new Size(1920, Height / scale));
            // 释放之前的 Mat 和 Bitmap 对象
            _srcGreyMat?.Dispose();
            _srcMat?.Dispose();
            _srcBitmap?.Dispose();
            // 返回新的 ImageRegion 对象
            return new ImageRegion(newMat, 0, 0, this, new ScaleConverter(scale));
            // return new ImageRegion(newMat, 0, 0, this, new TranslationConverter(0, 0));
        }

        /// <summary>
        /// 静态方法,在游戏窗体捕获区域维度进行点击
        /// </summary>
        /// <param name="posFunc">
        /// 实现一个方法输出要点击的的坐标(相对游戏捕获区域内坐标),提供以下参数供计算使用
        /// Size = 当前游戏捕获区域大小
        /// double = 当前游戏捕获区域到 1080P 的缩放比例
        /// 也就是说方法内的魔法数字必须是 1080P下的数字
        /// </param>
        public static void GameRegionClick(Func<Size, double, (double, double)> posFunc)
    # 获取系统信息中捕获区域的矩形
    var captureAreaRect = TaskContext.Instance().SystemInfo.CaptureAreaRect;
    # 获取系统信息中缩放比例
    var assetScale = TaskContext.Instance().SystemInfo.ScaleTo1080PRatio;
    # 根据捕获区域的尺寸和缩放比例计算点击坐标
    var (cx, cy) = posFunc(new Size(captureAreaRect.Width, captureAreaRect.Height), assetScale);
    # 在计算出的坐标位置点击
    DesktopRegion.DesktopRegionClick(captureAreaRect.X + cx, captureAreaRect.Y + cy);



    public static void GameRegionMove(Func<Size, double, (double, double)> posFunc)
    {
        # 获取系统信息中捕获区域的矩形
        var captureAreaRect = TaskContext.Instance().SystemInfo.CaptureAreaRect;
        # 获取系统信息中缩放比例
        var assetScale = TaskContext.Instance().SystemInfo.ScaleTo1080PRatio;
        # 根据捕获区域的尺寸和缩放比例计算移动坐标
        var (cx, cy) = posFunc(new Size(captureAreaRect.Width, captureAreaRect.Height), assetScale);
        # 在计算出的坐标位置移动
        DesktopRegion.DesktopRegionMove(captureAreaRect.X + cx, captureAreaRect.Y + cy);
    }



    /// <summary>
    /// 静态方法,输入1080P下的坐标,方法会自动转换到当前游戏捕获区域大小下的坐标并点击
    /// </summary>
    /// <param name="cx"></param>
    /// <param name="cy"></param>
    public static void GameRegion1080PPosClick(double cx, double cy)
    {
        # 将1080P坐标转换为实际游戏窗口坐标，并调用点击方法
        GameRegionClick((_, scale) => (cx * scale, cy * scale));
    }



    public static void GameRegion1080PPosMove(double cx, double cy)
    {
        # 将1080P坐标转换为实际游戏窗口坐标，并调用移动方法
        GameRegionMove((_, scale) => (cx * scale, cy * scale));
    }
请提供代码内容以便我为其添加注释。
```