# `.\better-genshin-impact\BetterGenshinImpact\Core\Recognition\RecognitionObject.cs`

```cs
﻿using BetterGenshinImpact.Core.Recognition.OpenCv;  // 引入 OpenCV 相关功能的命名空间
using BetterGenshinImpact.Helpers.Extensions;  // 引入扩展方法的命名空间
using OpenCvSharp;  // 引入 OpenCVSharp 的核心功能
using System;  // 引入系统基础功能
using System.Collections.Generic;  // 引入泛型集合功能
using System.Drawing;  // 引入图形功能

namespace BetterGenshinImpact.Core.Recognition;  // 定义命名空间

/// <summary>
///     识别对象
/// </summary>
[Serializable]  // 表示该类可以被序列化
public class RecognitionObject
{
    public RecognitionTypes RecognitionType { get; set; }  // 识别类型

    /// <summary>
    ///     感兴趣的区域
    /// </summary>
    public Rect RegionOfInterest { get; set; }  // 识别的兴趣区域

    public string? Name { get; set; }  // 识别对象的名称

    #region 模板匹配

    /// <summary>
    ///     模板匹配的对象(彩色)
    /// </summary>
    public Mat? TemplateImageMat { get; set; }  // 彩色模板图像

    /// <summary>
    ///     模板匹配的对象(灰色)
    /// </summary>
    public Mat? TemplateImageGreyMat { get; set; }  // 灰度模板图像

    /// <summary>
    ///     模板匹配阈值。可选，默认 0.8 。
    /// </summary>
    public double Threshold { get; set; } = 0.8;  // 模板匹配的阈值

    /// <summary>
    ///     是否使用 3 通道匹配。可选，默认 false 。
    /// </summary>
    public bool Use3Channels { get; set; } = false;  // 是否使用三通道匹配

    /// <summary>
    ///     模板匹配算法。可选，默认 CCoeffNormed 。
    ///     https://docs.opencv.org/4.x/df/dfb/group__imgproc__object.html
    /// </summary>
    public TemplateMatchModes TemplateMatchMode { get; set; } = TemplateMatchModes.CCoeffNormed;  // 模板匹配模式

    /// <summary>
    ///     匹配模板遮罩，指定图片中的某种色彩不需要匹配
    ///     使用时，需要将模板图片的背景色设置为纯绿色，即 (0, 255, 0)
    /// </summary>
    public bool UseMask { get; set; } = false;  // 是否使用遮罩

    /// <summary>
    ///     不需要匹配的颜色，默认绿色
    ///     UseMask = true 的时候有用
    /// </summary>
    public Color MaskColor { get; set; } = Color.FromArgb(0, 255, 0);  // 遮罩颜色

    public Mat? MaskMat { get; set; }  // 遮罩图像

    /// <summary>
    ///     匹配成功时，是否在屏幕上绘制矩形框。可选，默认 false 。
    ///     true 时 Name 必须有值。
    /// </summary>
    public bool DrawOnWindow { get; set; } = false;  // 是否在窗口上绘制矩形框

    /// <summary>
    ///     DrawOnWindow 为 true 时，绘制的矩形框的颜色。可选，默认红色。
    /// </summary>
    public Pen DrawOnWindowPen = new(Color.Red, 2);  // 矩形框的颜色和宽度

    /// <summary>
    ///    一个模板匹配多个结果的时候最大匹配数量。可选，默认 -1，即不限制。
    /// </summary>
    public int MaxMatchCount { get; set; } = -1;  // 最大匹配数量，-1 表示不限制

    public RecognitionObject InitTemplate()
    {
        if (TemplateImageMat != null && TemplateImageGreyMat == null)
        {
            TemplateImageGreyMat = new Mat();  // 如果没有灰度图像，则创建一个新的
            Cv2.CvtColor(TemplateImageMat, TemplateImageGreyMat, ColorConversionCodes.BGR2GRAY);  // 将彩色图像转换为灰度图像
        }

        if (UseMask && TemplateImageMat != null && MaskMat == null) MaskMat = OpenCvCommonHelper.CreateMask(TemplateImageMat, MaskColor.ToScalar());  // 如果使用遮罩且遮罩图像为空，则创建遮罩
        return this;  // 返回当前对象
    }

    #endregion 模板匹配

    #region 颜色匹配

    /// <summary>
    ///     颜色匹配方式。即 cv::ColorConversionCodes。可选，默认 4 (RGB)。
    ///     常用值：4 (RGB, 3 通道), 40 (HSV, 3 通道), 6 (GRAY, 1 通道)。
    ///     https://docs.opencv.org/4.x/d8/d01/group__imgproc__color__conversions.html
    /// </summary>
    public ColorConversionCodes ColorConversionCode { get; set; } = ColorConversionCodes.BGR2RGB;  // 颜色转换代码

    public Scalar LowerColor { get; set; }  // 颜色匹配的下限
    # 声明一个公共属性 UpperColor，类型为 Scalar，支持 get 和 set 操作
    public Scalar UpperColor { get; set; }

    /// <summary>
    ///     符合的点的数量要求。可选，默认 1
    /// </summary>
    # 声明一个公共属性 MatchCount，类型为 int，表示符合条件的点的数量要求，默认值为 1
    public int MatchCount { get; set; } = 1;

    #endregion 颜色匹配

    #region OCR文字识别

    /// <summary>
    ///     OCR 引擎。可选，只有 Paddle。
    /// </summary>
    # 声明一个公共属性 OcrEngine，类型为 OcrEngineTypes，表示 OCR 引擎，默认值为 OcrEngineTypes.Paddle
    public OcrEngineTypes OcrEngine { get; set; } = OcrEngineTypes.Paddle;

    /// <summary>
    ///     部分文字识别结果不准确，进行替换。可选。
    /// </summary>
    # 声明一个公共属性 ReplaceDictionary，类型为 Dictionary<string, string[]>，用于存储替换字典
    # 默认为空数组，表示没有替换规则
    public Dictionary<string, string[]> ReplaceDictionary { get; set; } = [];

    /// <summary>
    ///     包含匹配
    ///     多个值全匹配的情况下才算成功
    ///     复杂情况请用下面的正则匹配
    /// </summary>
    # 声明一个公共属性 AllContainMatchText，类型为 List<string>，用于存储包含匹配的文本列表
    # 所有值都匹配才算成功
    public List<string> AllContainMatchText { get; set; } = [];

    /// <summary>
    ///     包含匹配
    ///     一个值匹配就算成功
    /// </summary>
    # 声明一个公共属性 OneContainMatchText，类型为 List<string>，用于存储包含匹配的文本列表
    # 只要一个值匹配就算成功
    public List<string> OneContainMatchText { get; set; } = [];

    /// <summary>
    ///     正则匹配
    ///     多个值全匹配的情况下才算成功
    /// </summary>
    # 声明一个公共属性 RegexMatchText，类型为 List<string>，用于存储正则匹配的文本列表
    # 所有值都匹配才算成功
    public List<string> RegexMatchText { get; set; } = [];

    #endregion OCR文字识别
你提供的代码似乎是一个结尾符号，可能缺少了上下文。你能否提供更完整的代码片段，或者说明这个代码在什么样的代码块中使用？这样我可以更好地帮助你。
```