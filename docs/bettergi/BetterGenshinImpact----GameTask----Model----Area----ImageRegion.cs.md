# `.\better-genshin-impact\BetterGenshinImpact\GameTask\Model\Area\ImageRegion.cs`

```cs
# 引入 BetterGenshinImpact.Core.Recognition 命名空间
﻿using BetterGenshinImpact.Core.Recognition;
# 引入 OCR 相关的命名空间
using BetterGenshinImpact.Core.Recognition.OCR;
# 引入 OpenCV 相关的命名空间
using BetterGenshinImpact.Core.Recognition.OpenCv;
# 引入 GameTask.Common 命名空间
using BetterGenshinImpact.GameTask.Common;
# 引入 Area.Converter 模型的命名空间
using BetterGenshinImpact.GameTask.Model.Area.Converter;
# 引入 Helper 工具类的命名空间
using BetterGenshinImpact.Helpers;
# 引入 Drawable 视图相关的命名空间
using BetterGenshinImpact.View.Drawable;
# 引入日志记录功能的命名空间
using Microsoft.Extensions.Logging;
# 引入 OpenCVSharp 的命名空间
using OpenCvSharp;
# 引入 OpenCVSharp 扩展的命名空间
using OpenCvSharp.Extensions;
# 引入基础 .NET 类
using System;
using System.Collections.Generic;
using System.Drawing;
using System.Linq;
using System.Text.RegularExpressions;
# 引入 OpenCVSharp.Point 类型
using Point = OpenCvSharp.Point;

# 定义命名空间 BetterGenshinImpact.GameTask.Model.Area
namespace BetterGenshinImpact.GameTask.Model.Area;

# 定义 ImageRegion 类继承自 Region 类
public class ImageRegion : Region
{
    # 定义可空 Bitmap 对象
    protected Bitmap? _srcBitmap;
    # 定义可空 Mat 对象
    protected Mat? _srcMat;
    # 定义可空灰度 Mat 对象
    protected Mat? _srcGreyMat;

    # 定义 SrcBitmap 属性
    public Bitmap SrcBitmap
    {
        get
        {
            # 如果 _srcBitmap 不为空，则返回其值
            if (_srcBitmap != null)
            {
                return _srcBitmap;
            }

            # 如果 _srcMat 为空，则抛出异常
            if (_srcMat == null)
            {
                throw new Exception("SrcBitmap和SrcMat不能同时为空");
            }

            # 将 _srcMat 转换为 Bitmap 并赋值给 _srcBitmap
            _srcBitmap = _srcMat.ToBitmap();
            return _srcBitmap;
        }
    }

    # 定义 SrcMat 属性
    public Mat SrcMat
    {
        get
        {
            # 如果 _srcMat 不为空，则返回其值
            if (_srcMat != null)
            {
                return _srcMat;
            }

            # 如果 _srcBitmap 为空，则抛出异常
            if (_srcBitmap == null)
            {
                throw new Exception("SrcBitmap和SrcMat不能同时为空");
            }

            # 将 _srcBitmap 转换为 Mat 并赋值给 _srcMat
            _srcMat = _srcBitmap.ToMat();
            return _srcMat;
        }
    }

    # 定义 SrcGreyMat 属性
    public Mat SrcGreyMat
    {
        get
        {
            # 如果 _srcGreyMat 为空，则初始化为新 Mat 对象
            _srcGreyMat ??= new Mat();
            # 将 SrcMat 转换为灰度图像并存储到 _srcGreyMat
            Cv2.CvtColor(SrcMat, _srcGreyMat, ColorConversionCodes.BGR2GRAY);
            return _srcGreyMat;
        }
    }

    # 定义构造函数，接受 Bitmap 对象
    public ImageRegion(Bitmap bitmap, int x, int y, Region? owner = null, INodeConverter? converter = null) : base(x, y, bitmap.Width, bitmap.Height, owner, converter)
    {
        # 初始化 _srcBitmap
        _srcBitmap = bitmap;
    }

    # 定义构造函数，接受 Mat 对象
    public ImageRegion(Mat mat, int x, int y, Region? owner = null, INodeConverter? converter = null) : base(x, y, mat.Width, mat.Height, owner, converter)
    {
        # 初始化 _srcMat
        _srcMat = mat;
    }

    # 定义 HasImage 方法，检查是否有图像
    private bool HasImage()
    {
        return _srcBitmap != null || _srcMat != null;
    }

    # 定义 DeriveCrop 方法，通过裁剪源图像生成新区域
    public ImageRegion DeriveCrop(int x, int y, int w, int h)
    {
        # 根据裁剪区域创建新的 Mat 对象，并返回新的 ImageRegion
        return new ImageRegion(new Mat(SrcMat, new Rect(x, y, w, h)), x, y, this, new TranslationConverter(x, y));
    }

    # 定义重载 DeriveCrop 方法，接受 Rect 对象
    public ImageRegion DeriveCrop(Rect rect)
    {
        # 调用 DeriveCrop 方法，传递 Rect 的各个属性
        return DeriveCrop(rect.X, rect.Y, rect.Width, rect.Height);
    }

    # 定义 Derive 方法，已被注释
    // public ImageRegion Derive(Mat mat, int x, int y)
    // {
    //     return new ImageRegion(mat, x, y, this, new TranslationConverter(x, y));
    // }

    # 定义识别函数的占位符
    /// <summary>
    /// 在本区域内查找最优识别对象
    /// 或者对该区域进行识别
    /// 匹配
    /// RecognitionTypes.TemplateMatch
    /// RecognitionTypes.OcrMatch
    /// 识别
    /// RecognitionTypes.Ocr
    /// </summary>
    /// <param name="ro"></param>
    /// <param name="successAction">成功找到后做什么</param>
    /// <param name="failAction">失败后做什么</param>
    /// <returns>返回最优的一个识别结果RectArea</returns>
    /// <exception cref="Exception"></exception>
    public Region Find(RecognitionObject ro, Action<Region>? successAction = null, Action? failAction = null)
    {
        // 方法签名: 查找最优识别区域，并在成功或失败时执行相应的操作
    }

    /// <summary>
    /// 在本区域内查找识别对象
    /// 返回所有找到的结果
    /// 仅支持:
    /// RecognitionTypes.TemplateMatch
    /// RecognitionTypes.Ocr
    /// </summary>
    /// <param name="ro"></param>
    /// <param name="successAction">成功找到后做什么</param>
    /// <param name="failAction">失败后做什么</param>
    /// <returns>无内嵌图片的 RectArea List</returns>
    /// <exception cref="Exception"></exception>
    public List<Region> FindMulti(RecognitionObject ro, Action<List<Region>>? successAction = null, Action? failAction = null)
    {
        // 方法签名: 查找多个识别区域，并在成功或失败时执行相应的操作
    }

    public new void Dispose()
    {
        // 释放资源: 释放灰度图像矩阵的资源
        _srcGreyMat?.Dispose();
        // 释放资源: 释放源图像矩阵的资源
        _srcMat?.Dispose();
        // 释放资源: 释放源位图的资源
        _srcBitmap?.Dispose();
    }
# 结束当前代码块或结构
}
```