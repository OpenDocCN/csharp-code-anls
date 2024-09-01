# `.\better-genshin-impact\BetterGenshinImpact\GameTask\Model\Area\Region.cs`

```cs
﻿using BetterGenshinImpact.GameTask.Model.Area.Converter;  // 引入转换器相关的命名空间
using BetterGenshinImpact.View.Drawable;  // 引入视图绘制相关的命名空间
using OpenCvSharp;  // 引入 OpenCV 库的命名空间
using System;  // 引入系统相关的命名空间
using System.Diagnostics;  // 引入调试相关的命名空间
using System.Drawing;  // 引入绘图相关的命名空间
using System.Threading;  // 引入线程操作相关的命名空间
using Vanara.PInvoke;  // 引入 Vanara P/Invoke 的命名空间

namespace BetterGenshinImpact.GameTask.Model.Area
{
    /// <summary>
    /// 区域基类
    /// 用于描述一个区域，可以是一个矩形，也可以是一个点
    /// </summary>
    public class Region : IDisposable
    {
        public int X { get; set; }  // 区域的 X 坐标
        public int Y { get; set; }  // 区域的 Y 坐标
        public int Width { get; set; }  // 区域的宽度
        public int Height { get; set; }  // 区域的高度

        public int Top
        {
            get => Y;  // 获取区域的上边界，即 Y 坐标
            set => Y = value;  // 设置区域的上边界，即 Y 坐标
        }

        /// <summary>
        /// Gets the y-coordinate that is the sum of the Y and Height property values of this Rect structure.
        /// </summary>
        public int Bottom => Y + Height;  // 获取区域的下边界，即 Y 坐标加上高度

        /// <summary>
        /// Gets the x-coordinate of the left edge of this Rect structure.
        /// </summary>
        public int Left
        {
            get => X;  // 获取区域的左边界，即 X 坐标
            set => X = value;  // 设置区域的左边界，即 X 坐标
        }

        /// <summary>
        /// Gets the x-coordinate that is the sum of X and Width property values of this Rect structure.
        /// </summary>
        public int Right => X + Width;  // 获取区域的右边界，即 X 坐标加上宽度

        /// <summary>
        /// 存放OCR识别的结果文本
        /// </summary>
        public string Text { get; set; } = string.Empty;  // 区域内 OCR 识别的文本结果，默认为空字符串

        public Region()
        {
        }

        public Region(int x, int y, int width, int height, Region? owner = null, INodeConverter? converter = null)
        {
            X = x;  // 设置区域的 X 坐标
            Y = y;  // 设置区域的 Y 坐标
            Width = width;  // 设置区域的宽度
            Height = height;  // 设置区域的高度
            Prev = owner;  // 设置上一个区域节点
            PrevConverter = converter;  // 设置上一个区域节点的转换器
        }

        public Region(Rect rect, Region? owner = null, INodeConverter? converter = null) : this(rect.X, rect.Y, rect.Width, rect.Height, owner, converter)
        {
        }

        public Region? Prev { get; }  // 上一个区域节点

        /// <summary>
        /// 本区域节点向上一个区域节点坐标的转换器
        /// </summary>
        public INodeConverter? PrevConverter { get; }  // 上一个区域节点坐标的转换器

        // public List<Region>? NextChildren { get; protected set; }  // （被注释掉）下一子区域列表

        /// <summary>
        /// 后台点击【自己】的中心
        /// </summary>
        public void BackgroundClick()
        {
            User32.GetCursorPos(out var p);  // 获取当前鼠标指针的位置
            this.Move();  // 移动鼠标到区域的中心
            TaskContext.Instance().PostMessageSimulator.LeftButtonClickBackground();  // 模拟点击操作
            Thread.Sleep(10);  // 等待 10 毫秒
            DesktopRegion.DesktopRegionMove(p.X, p.Y);  // 将鼠标移动回原位置
        }

        /// <summary>
        /// 点击【自己】的中心
        /// region.Derive(x,y).Click() 等效于 region.ClickTo(x,y)
        /// </summary>
        public void Click()
        {
            // 相对自己是 0, 0 坐标
            ClickTo(0, 0, Width, Height);  // 点击区域的中心
        }

        /// <summary>
        /// 点击区域内【指定位置】
        /// region.Derive(x,y).Click() 等效于 region.ClickTo(x,y)
        /// </summary>
        /// <param name="x"></param>
        /// <param name="y"></param>
        public void ClickTo(int x, int y)
        {
            ClickTo(x, y, 0, 0);  // 点击指定位置
        }

        /// <summary>
        /// 点击区域内【指定矩形区域】的中心
        /// </summary>
        /// <param name="x"></param>
        /// <param name="y"></param>
        /// <param name="w"></param>
        /// <param name="h"></param>
        /// <exception cref="Exception"></exception>
    public void ClickTo(int x, int y, int w, int h)
    {
        // 将给定的位置和尺寸转换为目标区域，并点击该区域
        var res = ConvertRes<DesktopRegion>.ConvertPositionToTargetRegion(x, y, w, h, this);
        // 在转换后的目标区域进行点击操作
        res.TargetRegion.DesktopRegionClick(res.X, res.Y, res.Width, res.Height);
    }

    /// <summary>
    /// 移动到【自己】的中心
    /// region.Derive(x,y).Move() 等效于 region.MoveTo(x,y)
    /// </summary>
    public void Move()
    {
        // 移动到相对自身的 (0,0) 坐标，尺寸为自身的宽高
        MoveTo(0, 0, Width, Height);
    }

    /// <summary>
    /// 移动到区域内【指定位置】
    /// region.Derive(x,y).Move() 等效于 region.MoveTo(x,y)
    /// </summary>
    /// <param name="x"></param>
    /// <param name="y"></param>
    public void MoveTo(int x, int y)
    {
        // 移动到区域内的指定位置，默认宽高为0
        MoveTo(x, y, 0, 0);
    }

    /// <summary>
    /// 移动到区域内【指定矩形区域】的中心
    /// </summary>
    /// <param name="x"></param>
    /// <param name="y"></param>
    /// <param name="w"></param>
    /// <param name="h"></param>
    /// <exception cref="Exception"></exception>
    public void MoveTo(int x, int y, int w, int h)
    {
        // 将指定的矩形区域转换为目标区域，并移动到该区域中心
        var res = ConvertRes<DesktopRegion>.ConvertPositionToTargetRegion(x, y, w, h, this);
        // 在转换后的目标区域执行移动操作
        res.TargetRegion.DesktopRegionMove(res.X, res.Y, res.Width, res.Height);
    }

    /// <summary>
    /// 直接在遮罩窗口绘制【自己】
    /// </summary>
    /// <param name="name"></param>
    /// <param name="pen"></param>
    public void DrawSelf(string name, Pen? pen = null)
    {
        // 在遮罩窗口中绘制自身的矩形区域
        DrawRect(0, 0, Width, Height, name, pen);
    }

    /// <summary>
    /// 直接在遮罩窗口绘制当前区域下的【指定区域】
    /// </summary>
    /// <param name="x"></param>
    /// <param name="y"></param>
    /// <param name="w"></param>
    /// <param name="h"></param>
    /// <param name="name"></param>
    /// <param name="pen"></param>
    public void DrawRect(int x, int y, int w, int h, string name, Pen? pen = null)
    {
        // 将指定区域转换为可绘制对象，并在遮罩窗口中绘制该区域
        var drawable = ToRectDrawable(x, y, w, h, name, pen);
        VisionContext.Instance().DrawContent.PutRect(name, drawable);
    }

    public void DrawRect(Rect rect, string name, Pen? pen = null)
    {
        // 将指定的矩形区域转换为可绘制对象，并在遮罩窗口中绘制该区域
        var drawable = ToRectDrawable(rect.X, rect.Y, rect.Width, rect.Height, name, pen);
        VisionContext.Instance().DrawContent.PutRect(name, drawable);
    }

    /// <summary>
    /// 转换【自己】到遮罩窗口绘制矩形
    /// </summary>
    /// <param name="name"></param>
    /// <param name="pen"></param>
    /// <returns></returns>
    public RectDrawable SelfToRectDrawable(string name, Pen? pen = null)
    {
        // 将自身的矩形区域转换为可绘制对象
        return ToRectDrawable(0, 0, Width, Height, name, pen);
    }

    /// <summary>
    /// 转换【指定区域】到遮罩窗口绘制矩形
    /// </summary>
    /// <param name="rect"></param>
    /// <param name="name"></param>
    /// <param name="pen"></param>
    /// <returns></returns>
    public RectDrawable ToRectDrawable(Rect rect, string name, Pen? pen = null)
    {
        // 将指定矩形区域转换为可绘制对象
        return ToRectDrawable(rect.X, rect.Y, rect.Width, rect.Height, name, pen);
    }

    /// <summary>
    /// 转换【指定区域】到遮罩窗口绘制矩形
    /// </summary>
    /// <param name="x"></param>
    /// <param name="y"></param>
    /// <param name="w"></param>
    /// <param name="h"></param>
    /// <param name="name"></param>
    /// <param name="pen"></param>
    /// <returns></returns>
    /// <exception cref="Exception"></exception>
    // 将指定的坐标和尺寸转换为 RectDrawable 对象
    public RectDrawable ToRectDrawable(int x, int y, int w, int h, string name, Pen? pen = null)
    {
        // 将位置和尺寸转换为目标区域
        var res = ConvertRes<GameCaptureRegion>.ConvertPositionToTargetRegion(x, y, w, h, this);
        // 使用目标区域创建 RectDrawable 对象
        return res.TargetRegion.ConvertToRectDrawable(res.X, res.Y, res.Width, res.Height, pen, name);
    }

    /// <summary>
    /// 转换【指定直线】到遮罩窗口绘制直线
    /// </summary>
    /// <param name="x1"></param>
    /// <param name="y1"></param>
    /// <param name="x2"></param>
    /// <param name="y2"></param>
    /// <param name="name"></param>
    /// <param name="pen"></param>
    /// <returns></returns>
    // 将指定直线坐标转换为 LineDrawable 对象
    public LineDrawable ToLineDrawable(int x1, int y1, int x2, int y2, string name, Pen? pen = null)
    {
        // 将直线起点转换为目标区域
        var res1 = ConvertRes<GameCaptureRegion>.ConvertPositionToTargetRegion(x1, y1, 0, 0, this);
        // 将直线终点转换为目标区域
        var res2 = ConvertRes<GameCaptureRegion>.ConvertPositionToTargetRegion(x2, y2, 0, 0, this);
        // 使用转换后的坐标创建 LineDrawable 对象
        return res1.TargetRegion.ConvertToLineDrawable(res1.X, res1.Y, res2.X, res2.Y, pen, name);
    }

    // 绘制一条直线
    public void DrawLine(int x1, int y1, int x2, int y2, string name, Pen? pen = null)
    {
        // 将直线坐标转换为 LineDrawable 对象
        var drawable = ToLineDrawable(x1, y1, x2, y2, name, pen);
        // 将 LineDrawable 对象添加到绘图上下文中
        VisionContext.Instance().DrawContent.PutLine(name, drawable);
    }

    // 将当前对象的位置转换为游戏捕捉区域
    public Rect ConvertSelfPositionToGameCaptureRegion()
    {
        return ConvertPositionToGameCaptureRegion(X, Y, Width, Height);
    }

    // 将指定位置和尺寸转换为游戏捕捉区域
    public Rect ConvertPositionToGameCaptureRegion(int x, int y, int w, int h)
    {
        // 将位置和尺寸转换为目标区域
        var res = ConvertRes<GameCaptureRegion>.ConvertPositionToTargetRegion(x, y, w, h, this);
        // 返回目标区域的矩形表示
        return res.ToRect();
    }

    // 将指定位置转换为游戏捕捉区域的坐标
    public (int, int) ConvertPositionToGameCaptureRegion(int x, int y)
    {
        // 将位置转换为目标区域
        var res = ConvertRes<GameCaptureRegion>.ConvertPositionToTargetRegion(x, y, 0, 0, this);
        // 返回目标区域的坐标
        return (res.X, res.Y);
    }

    // 将指定位置转换为桌面区域的坐标
    public (int, int) ConvertPositionToDesktopRegion(int x, int y)
    {
        // 将位置转换为桌面区域
        var res = ConvertRes<DesktopRegion>.ConvertPositionToTargetRegion(x, y, 0, 0, this);
        // 返回桌面区域的坐标
        return (res.X, res.Y);
    }

    // 将当前对象的属性转换为矩形表示
    public Rect ToRect()
    {
        return new Rect(X, Y, Width, Height);
    }

    /// <summary>
    /// 生成一个新的区域
    /// 请使用 using var newRegion
    /// </summary>
    /// <returns></returns>
    // 创建一个新的 ImageRegion 对象
    public ImageRegion ToImageRegion()
    {
        // 如果当前对象已经是 ImageRegion，则返回当前对象
        if (this is ImageRegion imageRegion)
        {
            Debug.WriteLine("ToImageRegion 但已经是 ImageRegion");
            return imageRegion;
        }

        // 将当前位置和尺寸转换为目标区域，并创建新的 ImageRegion 对象
        var res = ConvertRes<ImageRegion>.ConvertPositionToTargetRegion(0, 0, Width, Height, this);
        var newRegion = new ImageRegion(new Mat(res.TargetRegion.SrcMat, res.ToRect()), X, Y, Prev, PrevConverter);
        return newRegion;
    }

    public bool IsEmpty()
    # 返回是否所有指定属性（宽度、高度、X 和 Y 坐标）均为 0
    return Width == 0 && Height == 0 && X == 0 && Y == 0;
    
    /// <summary>
    /// 语义化包装
    /// </summary>
    /// <returns></returns>
    // 返回当前对象是否存在（即非空）
    public bool IsExist()
    {
        // 调用 IsEmpty 方法判断对象是否为空，然后取反
        return !IsEmpty();
    }

    // 释放当前对象的资源
    public void Dispose()
    {
        // 子节点全部释放
        // NextChildren?.ForEach(x => x.Dispose());
        // 遍历 NextChildren 列表并对每个子节点调用 Dispose 方法（被注释掉的代码）
    }

    /// <summary>
    /// 派生一个点类型的区域
    /// </summary>
    /// <param name="x"></param>
    /// <param name="y"></param>
    /// <returns></returns>
    // 创建一个新的区域对象，宽度和高度为 0
    public Region Derive(int x, int y)
    {
        // 调用另一个 Derive 方法，传递 0 作为宽度和高度
        return Derive(x, y, 0, 0);
    }

    /// <summary>
    /// 派生一个矩形类型的区域
    /// </summary>
    /// <param name="x"></param>
    /// <param name="y"></param>
    /// <param name="w"></param>
    /// <param name="h"></param>
    /// <returns></returns>
    // 创建一个新的区域对象，使用指定的 x, y, 宽度和高度
    public Region Derive(int x, int y, int w, int h)
    {
        // 使用传入的参数创建并返回一个新的 Region 对象
        return new Region(x, y, w, h, this, new TranslationConverter(x, y));
    }

    // 使用矩形对象的属性创建一个新的区域对象
    public Region Derive(Rect rect)
    {
        // 调用另一个 Derive 方法，使用矩形的属性作为参数
        return Derive(rect.X, rect.Y, rect.Width, rect.Height);
    }
# 代码块结束的闭合大括号，标志着代码块的结束
}
```