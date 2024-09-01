# `.\better-genshin-impact\BetterGenshinImpact\GameTask\Model\RectArea.cs`

```cs
﻿// 使用 BetterGenshinImpact.Core.Recognition 命名空间中的类和方法
// using BetterGenshinImpact.Core.Recognition;
// 使用 BetterGenshinImpact.Core.Recognition.OCR 命名空间中的类和方法
// using BetterGenshinImpact.Core.Recognition.OCR;
// 使用 BetterGenshinImpact.Core.Recognition.OpenCv 命名空间中的类和方法
// using BetterGenshinImpact.Core.Recognition.OpenCv;
// 使用 BetterGenshinImpact.Helpers 命名空间中的类和方法
// using BetterGenshinImpact.Helpers;
// 使用 BetterGenshinImpact.Helpers.Extensions 命名空间中的扩展方法
// using BetterGenshinImpact.Helpers.Extensions;
// 使用 BetterGenshinImpact.View.Drawable 命名空间中的类和方法
// using BetterGenshinImpact.View.Drawable;
// 使用 OpenCvSharp 命名空间中的类和方法
// using OpenCvSharp;
// 使用 OpenCvSharp.Extensions 命名空间中的扩展方法
// using OpenCvSharp.Extensions;
// 使用 Sdcb.PaddleOCR 命名空间中的类和方法
// using Sdcb.PaddleOCR;
// 使用 System 命名空间中的类和方法
// using System;
// 使用 System.Collections.Generic 命名空间中的类和方法
// using System.Collections.Generic;
// 使用 System.Drawing 命名空间中的类和方法
// using System.Drawing;
// 使用 System.Linq 命名空间中的类和方法
// using System.Linq;
// 使用 System.Text.RegularExpressions 命名空间中的正则表达式类
// using System.Text.RegularExpressions;
// 使用 OpenCvSharp.Point 作为 Point 类型
// using Point = OpenCvSharp.Point;
//
// 声明命名空间 BetterGenshinImpact.GameTask.Model
// namespace BetterGenshinImpact.GameTask.Model;
//
// /// <summary>
// /// 描述屏幕上的矩形区域或点，用于图像识别和坐标转换
// /// 一般层级包括：桌面 -> 窗口捕获区域 -> 窗口内的矩形区域 -> 矩形区域内的图像区域
// /// </summary>
// [Serializable]
// public class RectArea : IDisposable
// {
//     /// <summary>
//     /// 定义当前区域的坐标系层级
//     /// 0 表示桌面，1 表示游戏窗口
//     /// 顶层默认为桌面
//     /// Desktop -> GameCaptureArea -> Part -> ?
//     /// </summary>
//     public int CoordinateLevelNum { get; set; } = 0;
//
//     // 区域的 X 坐标
//     public int X { get; set; }
//     // 区域的 Y 坐标
//     public int Y { get; set; }
//     // 区域的宽度
//     public int Width { get; set; }
//     // 区域的高度
//     public int Height { get; set; }
//
//     // 区域的拥有者，可能是一个父级区域
//     public RectArea? Owner { get; set; }
//
//     // 存储源图像的位图
//     private Bitmap? _srcBitmap;
//     // 存储源图像的 Mat 对象
//     private Mat? _srcMat;
//     // 存储源图像的灰度 Mat 对象
//     private Mat? _srcGreyMat;
//
//     // 获取源图像的位图，如果不存在则从 Mat 对象生成
//     public Bitmap SrcBitmap
//     {
//         get
//         {
//             if (_srcBitmap != null)
//             {
//                 return _srcBitmap;
//             }
//
//             if (_srcMat == null)
//             {
//                 throw new Exception("SrcBitmap和SrcMat不能同时为空");
//             }
//
//             _srcBitmap = _srcMat.ToBitmap();
//             return _srcBitmap;
//         }
//     }
//
//     // 获取源图像的 Mat 对象，如果不存在则从位图生成
//     public Mat SrcMat
//     {
//         get
//         {
//             if (_srcMat != null)
//             {
//                 return _srcMat;
//             }
//
//             if (_srcBitmap == null)
//             {
//                 throw new Exception("SrcBitmap和SrcMat不能同时为空");
//             }
//
//             _srcMat = _srcBitmap.ToMat();
//             return _srcMat;
//         }
//     }
//
//     // 获取源图像的灰度 Mat 对象，如果不存在则进行转换
//     public Mat SrcGreyMat
//     {
//         get
//         {
//             _srcGreyMat ??= new Mat();
//             Cv2.CvtColor(SrcMat, _srcGreyMat, ColorConversionCodes.BGR2GRAY);
//             return _srcGreyMat;
//         }
//     }
//
//     /// <summary>
//     /// 存储 OCR 识别的结果文本
//     /// </summary>
//     public string Text { get; set; } = string.Empty;
//
//     // 默认构造函数
//     public RectArea()
//     {
//     }
//
//     // 带坐标和大小的构造函数
//     public RectArea(int x, int y, int width, int height, RectArea? owner = null)
//     {
//         X = x;
//         Y = y;
//         Width = width;
//         Height = height;
//         Owner = owner;
//         CoordinateLevelNum = owner?.CoordinateLevelNum + 1 ?? 0;
//     }
//
//     // 带位图和坐标的构造函数
//     public RectArea(Bitmap bitmap, int x, int y, RectArea? owner = null) : this(x, y, 0, 0, owner)
//     {
        // 将 bitmap 对象赋值给私有字段 _srcBitmap
        _srcBitmap = bitmap;
        // 设置宽度属性为 bitmap 的宽度
        Width = bitmap.Width;
        // 设置高度属性为 bitmap 的高度
        Height = bitmap.Height;
        // 结束构造函数
    }

    // 构造函数：使用 Mat 对象和坐标 (x, y) 初始化 RectArea 对象，并指定可选的拥有者
    public RectArea(Mat mat, int x, int y, RectArea? owner = null) : this(x, y, 0, 0, owner)
    {
        // 将 Mat 对象赋值给私有字段 _srcMat
        _srcMat = mat;
        // 设置宽度属性为 Mat 的宽度
        Width = mat.Width;
        // 设置高度属性为 Mat 的高度
        Height = mat.Height;
    }

    // 构造函数：使用 Mat 对象和点 p (包含 x 和 y 坐标) 初始化 RectArea 对象，并指定可选的拥有者
    public RectArea(Mat mat, Point p, RectArea? owner = null) : this(mat, p.X, p.Y, owner)
    {
    }

    // 构造函数：使用 Rect 对象初始化 RectArea 对象，并指定可选的拥有者
    public RectArea(Rect rect, RectArea? owner = null) : this(rect.X, rect.Y, rect.Width, rect.Height, owner)
    {
    }

    // 注释掉的构造函数：根据 Mat 对象初始化 RectArea 对象
    //public RectArea(Mat mat, RectArea? owner = null)
    //{
    //    _srcMat = mat;
    //    X = 0;
    //    Y = 0;
    //    Width = mat.Width;
    //    Height = mat.Height;
    //    Owner = owner;
    //    CoordinateLevelNum = owner?.CoordinateLevelNum + 1 ?? 0;
    //}

    // 将相对位置转换为指定坐标级别的绝对位置
    public Rect ConvertRelativePositionTo(int coordinateLevelNum)
    {
        // 初始化新坐标
        int newX = X, newY = Y;
        // 从当前对象开始向上查找
        var father = Owner;
        while (true)
        {
            // 如果没有找到对应的坐标系，抛出异常
            if (father == null)
            {
                throw new Exception("找不到对应的坐标系");
            }

            // 如果找到的坐标系级别匹配目标级别，结束循环
            if (father.CoordinateLevelNum == coordinateLevelNum)
            {
                break;
            }

            // 更新新坐标
            newX += father.X;
            newY += father.Y;

            // 向上查找下一个坐标系
            father = father.Owner;
        }

        // 返回新的绝对坐标区域
        return new Rect(newX, newY, Width, Height);
    }

    // 将相对位置转换为桌面坐标
    public Rect ConvertRelativePositionToDesktop()
    {
        return ConvertRelativePositionTo(0);
    }

    // 将相对位置转换为捕获区域坐标
    public Rect ConvertRelativePositionToCaptureArea()
    {
        return ConvertRelativePositionTo(1);
    }

    // 将当前区域转换为 Rect 对象
    public Rect ToRect()
    {
        return new Rect(X, Y, Width, Height);
    }

    // 检查当前区域是否在桌面坐标系统中
    public bool PositionIsInDesktop()
    {
        return CoordinateLevelNum == 0;
    }

    // 检查当前区域是否为空
    public bool IsEmpty()
    {
        return Width == 0 && Height == 0 && X == 0 && Y == 0;
    }

    /// <summary>
    /// 语义化包装
    /// </summary>
    /// <returns></returns>
    // 检查当前区域是否存在
    public bool IsExist()
    {
        return !IsEmpty();
    }

    // 检查是否包含图像
    public bool HasImage()
    {
        return _srcBitmap != null || _srcMat != null;
    }

    /// <summary>
    /// 在本区域内查找最优识别对象
    /// </summary>
    /// <param name="ro"></param>
    /// <param name="successAction">成功找到后做什么</param>
    /// <param name="failAction">失败后做什么</param>
    /// <returns>返回最优的一个识别结果RectArea</returns>
    /// <exception cref="Exception"></exception>
    // 查找符合条件的识别对象
    public RectArea Find(RecognitionObject ro, Action<RectArea>? successAction = null, Action? failAction = null)
    {
        // 如果没有图像内容，抛出异常
        if (!HasImage())
        {
            throw new Exception("当前对象内没有图像内容，无法完成 Find 操作");
        }

        // 如果识别对象为空，执行相关操作
        if (ro == null)
        {
# 抛出异常，如果识别对象为空
//             throw new Exception("识别对象不能为null");
//         }
//
//         # 检查识别类型是否为模板匹配
//         if (RecognitionTypes.TemplateMatch.Equals(ro.RecognitionType))
//         {
//             # 声明变量用于存储图像区域和模板
//             Mat roi;
//             Mat? template;
//             
//             # 如果需要使用三通道图像
//             if (ro.Use3Channels)
//             {
//                 # 使用三通道模板图像
//                 template = ro.TemplateImageMat;
//                 # 设置待匹配的图像区域为源图像
//                 roi = SrcMat;
//                 # 将图像从 BGRA 转换为 BGR
//                 Cv2.CvtColor(roi, roi, ColorConversionCodes.BGRA2BGR);
//             }
//             else
//             {
//                 # 使用灰度模板图像
//                 template = ro.TemplateImageGreyMat;
//                 # 设置待匹配的图像区域为灰度图像
//                 roi = SrcGreyMat;
//             }
//
//             # 如果模板图像为空，抛出异常
//             if (template == null)
//             {
//                 throw new Exception($"[TemplateMatch]识别对象{ro.Name}的模板图片不能为null");
//             }
//
//             # 如果指定了感兴趣区域（ROI）
//             if (ro.RegionOfInterest != Rect.Empty)
//             {
//                 # TODO roi 是可以加缓存的
//                 # 对 ROI 的边界进行检查（代码被注释掉）
//                 // if (!(0 <= ro.RegionOfInterest.X && 0 <= ro.RegionOfInterest.Width && ro.RegionOfInterest.X + ro.RegionOfInterest.Width <= roi.Cols
//                 //       && 0 <= ro.RegionOfInterest.Y && 0 <= ro.RegionOfInterest.Height && ro.RegionOfInterest.Y + ro.RegionOfInterest.Height <= roi.Rows))
//                 // {
//                 //     Logger.LogError("输入图像{W1}x{H1},模板ROI位置{X2}x{Y2},区域{H2}x{W2},边界溢出！", roi.Width, roi.Height, ro.RegionOfInterest.X, ro.RegionOfInterest.Y, ro.RegionOfInterest.Width, ro.RegionOfInterest.Height);
//                 // }
//                 # 根据 ROI 从图像中裁剪出对应区域
//                 roi = new Mat(roi, ro.RegionOfInterest);
//             }
//
//             # 使用模板匹配算法查找模板在图像中的位置
//             var p = MatchTemplateHelper.MatchTemplate(roi, template, ro.TemplateMatchMode, ro.MaskMat, ro.Threshold);
//             
//             # 如果找到匹配的点
//             if (p != new Point())
//             {
//                 # 创建新的矩形区域对象，表示匹配结果
//                 var newRa = new RectArea(template.Clone(), p.X + ro.RegionOfInterest.X, p.Y + ro.RegionOfInterest.Y, this);
//                 
//                 # 如果需要在窗口上绘制匹配区域
//                 if (ro.DrawOnWindow && !string.IsNullOrEmpty(ro.Name))
//                 {
//                     VisionContext.Instance().DrawContent.PutRect(ro.Name, newRa
//                         .ConvertRelativePositionToCaptureArea()
//                         .ToRectDrawable(ro.DrawOnWindowPen, ro.Name));
//                 }
//
//                 # 调用成功回调函数
//                 successAction?.Invoke(newRa);
//                 # 返回匹配的矩形区域
//                 return newRa;
//             }
//             else
//             {
//                 # 如果没有找到匹配的点，且需要在窗口上绘制匹配区域
//                 if (ro.DrawOnWindow && !string.IsNullOrEmpty(ro.Name))
//                 {
//                     VisionContext.Instance().DrawContent.RemoveRect(ro.Name);
//                 }
//
//                 # 调用失败回调函数
//                 failAction?.Invoke();
//                 # 返回一个空的矩形区域
//                 return new RectArea();
//             }
//         }
//         # 检查识别类型是否为 OCR 匹配
//         else if (RecognitionTypes.OcrMatch.Equals(ro.RecognitionType))
//         {
//             # 检查 OCR 匹配文本集合是否为空
//             if (ro.AllContainMatchText.Count == 0 && ro.OneContainMatchText.Count == 0 && ro.RegexMatchText.Count == 0)
//             {
//                 throw new Exception($"[OCR]识别对象{ro.Name}的匹配文本不能全为空");
//             }
//
//             # 设置感兴趣区域为源灰度图像
//             var roi = SrcGreyMat;
# 检查区域是否为空，如果不为空则从源灰度图中截取感兴趣区域
//             if (ro.RegionOfInterest != Rect.Empty)
//             {
//                 roi = new Mat(SrcGreyMat, ro.RegionOfInterest);
//             }
//
//             # 获取OCR结果
//             var result = OcrFactory.Paddle.OcrResult(roi);
//             # 移除结果文本中的所有空格
//             var text = StringUtils.RemoveAllSpace(result.Text);
//             # 替换文本中的指定内容
//             foreach (var entry in ro.ReplaceDictionary)
//             {
//                 foreach (var replaceStr in entry.Value)
//                 {
//                     text = text.Replace(entry.Key, replaceStr);
//                 }
//             }
//
//             # 统计包含匹配文本的个数
//             int successContainCount = 0, successRegexCount = 0;
//             bool successOneContain = false;
//             # 检查文本是否包含所有必须的匹配文本
//             foreach (var s in ro.AllContainMatchText)
//             {
//                 if (text.Contains(s))
//                 {
//                     successContainCount++;
//                 }
//             }
//
//             # 检查文本是否包含至少一个可选匹配文本
//             foreach (var s in ro.OneContainMatchText)
//             {
//                 if (text.Contains(s))
//                 {
//                     successOneContain = true;
//                     break;
//                 }
//             }
//
//             # 使用正则表达式检查匹配文本的个数
//             foreach (var re in ro.RegexMatchText)
//             {
//                 if (Regex.IsMatch(text, re))
//                 {
//                     successRegexCount++;
//                 }
//             }
//
//             # 判断是否满足所有匹配条件
//             if (successContainCount == ro.AllContainMatchText.Count
//                 && successRegexCount == ro.RegexMatchText.Count
//                 && (ro.OneContainMatchText.Count == 0 || successOneContain))
//             {
//                 # 创建新的区域对象，并调整其位置
//                 var newRa = new RectArea(roi, X + ro.RegionOfInterest.X, Y + ro.RegionOfInterest.Y, this);
//                 # 如果需要在窗口上绘制内容，则绘制识别结果
//                 if (ro.DrawOnWindow && !string.IsNullOrEmpty(ro.Name))
//                 {
//                     VisionContext.Instance().DrawContent.PutOrRemoveRectList(ro.Name, result.ToRectDrawableListOffset(ro.RegionOfInterest.X, ro.RegionOfInterest.Y));
//                 }
//
//                 # 执行成功的回调操作，并返回新创建的区域对象
//                 successAction?.Invoke(newRa);
//                 return newRa;
//             }
//             else
//             {
//                 # 如果需要在窗口上绘制内容，则移除绘制的内容
//                 if (ro.DrawOnWindow && !string.IsNullOrEmpty(ro.Name))
//                 {
//                     VisionContext.Instance().DrawContent.RemoveRect(ro.Name);
//                 }
//
//                 # 执行失败的回调操作，并返回一个空的区域对象
//                 failAction?.Invoke();
//                 return new RectArea();
//             }
//         }
//         else
//         {
//             # 如果不支持的识别类型，则抛出异常
//             throw new Exception($"RectArea不支持的识别类型{ro.RecognitionType}");
//         }
//     }
//
//     /// <summary>
//     /// 在本区域内查找识别对象
//     /// 返回所有找到的结果
//     /// 仅支持:
//     /// RecognitionTypes.TemplateMatch
//     /// RecognitionTypes.Ocr
//     /// </summary>
//     /// <param name="ro"></param>
//     /// <param name="successAction">成功找到后做什么</param>
# 定义一个方法来查找多个矩形区域
public List<RectArea> FindMulti(RecognitionObject ro, Action<List<RectArea>>? successAction = null, Action? failAction = null)
{
    # 检查是否有图像内容
    if (!HasImage())
    {
        # 如果没有图像，抛出异常
        throw new Exception("当前对象内没有图像内容，无法完成 Find 操作");
    }

    # 检查识别对象是否为null
    if (ro == null)
    {
        # 如果识别对象为null，抛出异常
        throw new Exception("识别对象不能为null");
    }

    # 如果识别类型是模板匹配
    if (RecognitionTypes.TemplateMatch.Equals(ro.RecognitionType))
    {
        Mat roi;
        Mat? template;
        # 如果使用三通道图像
        if (ro.Use3Channels)
        {
            template = ro.TemplateImageMat;
            roi = SrcMat;
            # 将图像从BGRA转换为BGR
            Cv2.CvtColor(roi, roi, ColorConversionCodes.BGRA2BGR);
        }
        else
        {
            template = ro.TemplateImageGreyMat;
            roi = SrcGreyMat;
        }

        # 检查模板图像是否为null
        if (template == null)
        {
            # 如果模板图像为null，抛出异常
            throw new Exception($"[TemplateMatch]识别对象{ro.Name}的模板图片不能为null");
        }

        # 如果有感兴趣区域，则裁剪图像
        if (ro.RegionOfInterest != Rect.Empty)
        {
            # TODO roi 是可以加缓存的
            roi = new Mat(roi, ro.RegionOfInterest);
        }

        # 执行模板匹配，返回匹配到的矩形区域列表
        var rectList = MatchTemplateHelper.MatchOnePicForOnePic(roi, template, ro.TemplateMatchMode, ro.MaskMat, ro.Threshold);
        if (rectList.Count > 0)
        {
            # 将矩形区域列表调整为相对于原始图像的位置
            var resRaList = rectList.Select(r => this.Derive(r + new Point(ro.RegionOfInterest.X, ro.RegionOfInterest.Y))).ToList();

            # 如果需要在窗口上绘制结果
            if (ro.DrawOnWindow && !string.IsNullOrEmpty(ro.Name))
            {
                VisionContext.Instance().DrawContent.PutOrRemoveRectList(ro.Name,
                    resRaList.Select(ra => ra.ConvertRelativePositionToCaptureArea()
                        .ToRectDrawable(ro.DrawOnWindowPen, ro.Name)).ToList());
            }

            # 调用成功回调函数，并返回结果列表
            successAction?.Invoke(resRaList);
            return resRaList;
        }
        else
        {
            # 如果没有匹配结果，且需要在窗口上绘制结果
            if (ro.DrawOnWindow && !string.IsNullOrEmpty(ro.Name))
            {
                VisionContext.Instance().DrawContent.RemoveRect(ro.Name);
            }

            # 调用失败回调函数，并返回空列表
            failAction?.Invoke();
            return [];
        }
    }
    # 如果识别类型是 OCR
    else if (RecognitionTypes.Ocr.Equals(ro.RecognitionType))
    {
        var roi = SrcGreyMat;
        # 如果有感兴趣区域，则裁剪图像
        if (ro.RegionOfInterest != Rect.Empty)
        {
            roi = new Mat(SrcGreyMat, ro.RegionOfInterest);
        }

        # 使用 OCR 工厂进行识别
        var result = OcrFactory.Paddle.OcrResult(roi);

        # 如果有识别结果
# 使用选择器将每个区域的矩形调整到感兴趣区域并创建新的区域对象
//                 var resRaList = result.Regions.Select(r =>
//                 {
//                     # 通过调整区域的矩形位置，创建新的区域对象
//                     var newRa = this.Derive(r.Rect.BoundingRect() + new Point(ro.RegionOfInterest.X, ro.RegionOfInterest.Y));
//                     # 复制文本内容到新的区域对象
//                     newRa.Text = r.Text;
//                     # 返回新的区域对象
//                     return newRa;
//                 }).ToList();
//                 # 如果需要在窗口上绘制并且区域名称不为空
//                 if (ro.DrawOnWindow && !string.IsNullOrEmpty(ro.Name))
//                 {
//                     # 在窗口上绘制矩形区域
//                     VisionContext.Instance().DrawContent.PutOrRemoveRectList(ro.Name, result.ToRectDrawableListOffset(ro.RegionOfInterest.X, ro.RegionOfInterest.Y));
//                 }
//
//                 # 调用成功操作的回调函数，并传入处理后的区域列表
//                 successAction?.Invoke(resRaList);
//                 # 返回处理后的区域列表
//                 return resRaList;
//             }
//             else
//             {
//                 # 如果需要在窗口上绘制并且区域名称不为空
//                 if (ro.DrawOnWindow && !string.IsNullOrEmpty(ro.Name))
//                 {
//                     # 从窗口中移除矩形区域
//                     VisionContext.Instance().DrawContent.RemoveRect(ro.Name);
//                 }
//
//                 # 调用失败操作的回调函数
//                 failAction?.Invoke();
//                 # 返回一个空数组
//                 return [];
//             }
//         }
//         else
//         {
//             # 抛出异常，表示不支持的识别类型
//             throw new Exception($"RectArea多目标识别不支持的识别类型{ro.RecognitionType}");
//         }
//     }
//
//     /// <summary>
//     /// 找到识别对象并点击中心
//     /// </summary>
//     /// <param name="ro"></param>
//     /// <returns></returns>
//     public RectArea ClickCenter(RecognitionObject ro)
//     {
//         # 查找与识别对象匹配的区域
//         var ra = Find(ro);
//         # 如果区域不为空，点击其中心
//         if (!ra.IsEmpty())
//         {
//             ra.ClickCenter();
//         }
//
//         # 返回找到的区域
//         return ra;
//     }
//
//     /// <summary>
//     /// 当前对象点击中心
//     /// </summary>
//     public void ClickCenter()
//     {
//         # 如果坐标系在桌面级别，点击区域的中心
//         if (CoordinateLevelNum == 0)
//         {
//             ToRect().ClickCenter();
//         }
//         else
//         {
//             # 否则，将相对位置转换为桌面坐标再点击中心
//             ConvertRelativePositionToDesktop().ClickCenter();
//         }
//     }
//
//     /// <summary>
//     /// 剪裁图片
//     /// </summary>
//     /// <param name="rect"></param>
//     /// <returns></returns>
//     public RectArea Crop(Rect rect)
//     {
//         # 根据给定的矩形裁剪图片，并返回新的区域对象
//         return new RectArea(SrcMat[rect], rect.X, rect.Y, this);
//     }
//
//     /// <summary>
//     /// 派生区域（无图片）
//     /// </summary>
//     /// <param name="rect"></param>
//     /// <returns></returns>
//     public RectArea Derive(Rect rect)
//     {
//         # 根据给定的矩形创建新的区域对象（不包含图片）
//         return new RectArea(rect, this);
//     }
//
//     /// <summary>
//     /// 派生2x2区域（无图片）
//     /// 方便用于点击
//     /// </summary>
//     /// <param name="x"></param>
//     /// <param name="y"></param>
//     /// <returns></returns>
//     public RectArea DerivePoint(int x, int y)
//     {
//         # 创建一个 2x2 的区域对象，用于点击操作
//         return new RectArea(new Rect(x, y, 2, 2), this);
//     }
//
//     /// <summary>
//     /// OCR识别
//     /// </summary>
//     /// <returns>所有结果</returns>
//     public PaddleOcrResult OcrResult()
//     {
//         # 使用 OCR 工厂获取图像的 OCR 结果
//         return OcrFactory.Paddle.OcrResult(SrcGreyMat);
//     }
//
//     public void Dispose()
//     {
# 释放 _srcGreyMat 对象占用的资源（如果存在）
_srcGreyMat?.Dispose();
// 释放 _srcMat 对象占用的资源（如果存在）
_srcMat?.Dispose();
// 释放 _srcBitmap 对象占用的资源（如果存在）
_srcBitmap?.Dispose();
// 结束注释块
// }
```