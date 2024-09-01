# `.\better-genshin-impact\BetterGenshinImpact\GameTask\AutoFishing\Assets\AutoFishingAssets.cs`

```cs
# 引用 BetterGenshinImpact.Core.Recognition 命名空间中的内容
﻿using BetterGenshinImpact.Core.Recognition;
# 引用 BetterGenshinImpact.GameTask.Model 命名空间中的内容
using BetterGenshinImpact.GameTask.Model;
# 引用 OpenCvSharp 命名空间中的内容
using OpenCvSharp;

# 定义 BetterGenshinImpact.GameTask.AutoFishing.Assets 命名空间
namespace BetterGenshinImpact.GameTask.AutoFishing.Assets;

# 定义 AutoFishingAssets 类，继承自 BaseAssets<AutoFishingAssets>
public class AutoFishingAssets : BaseAssets<AutoFishingAssets>
{
    # 声明 RecognitionObject 类型的公共字段 SpaceButtonRo
    public RecognitionObject SpaceButtonRo;

    # 声明多个 RecognitionObject 类型的公共字段，用于不同按钮的识别
    public RecognitionObject BaitButtonRo;
    public RecognitionObject WaitBiteButtonRo;
    public RecognitionObject LiftRodButtonRo;
    public RecognitionObject ExitFishingButtonRo;

    # 定义构造函数，构造函数是私有的
    private AutoFishingAssets()
    # 创建一个 RecognitionObject 实例，用于识别 SpaceButton
    {
        SpaceButtonRo = new RecognitionObject
        {
            # 设置识别对象的名称
            Name = "SpaceButton",
            # 设置识别类型为模板匹配
            RecognitionType = RecognitionTypes.TemplateMatch,
            # 加载并设置模板图像
            TemplateImageMat = GameTaskManager.LoadAssetImage("AutoFishing", "space.png"),
            # 设置识别区域的矩形范围
            RegionOfInterest = new Rect(CaptureRect.Width - CaptureRect.Width / 3,
                CaptureRect.Height - CaptureRect.Height / 5,
                CaptureRect.Width / 3,
                CaptureRect.Height / 5),
            # 设置是否在窗口上绘制识别区域
            DrawOnWindow = false
        }.InitTemplate();  # 初始化模板对象

        # 创建一个 RecognitionObject 实例，用于识别 BaitButton
        BaitButtonRo = new RecognitionObject
        {
            # 设置识别对象的名称
            Name = "BaitButton",
            # 设置识别类型为模板匹配
            RecognitionType = RecognitionTypes.TemplateMatch,
            # 加载并设置模板图像
            TemplateImageMat = GameTaskManager.LoadAssetImage("AutoFishing", "switch_bait.png"),
            # 设置识别区域的矩形范围
            RegionOfInterest = new Rect(CaptureRect.Width - CaptureRect.Width / 2,
                CaptureRect.Height - CaptureRect.Height / 4,
                CaptureRect.Width / 2,
                CaptureRect.Height / 4),
            # 设置匹配的阈值
            Threshold = 0.7,
            # 设置是否在窗口上绘制识别区域
            DrawOnWindow = false
        }.InitTemplate();  # 初始化模板对象

        # 创建一个 RecognitionObject 实例，用于识别 WaitBiteButton
        WaitBiteButtonRo = new RecognitionObject
        {
            # 设置识别对象的名称
            Name = "WaitBiteButton",
            # 设置识别类型为模板匹配
            RecognitionType = RecognitionTypes.TemplateMatch,
            # 加载并设置模板图像
            TemplateImageMat = GameTaskManager.LoadAssetImage("AutoFishing", "wait_bite.png"),
            # 设置识别区域的矩形范围
            RegionOfInterest = new Rect(CaptureRect.Width - CaptureRect.Width / 2,
                CaptureRect.Height - CaptureRect.Height / 4,
                CaptureRect.Width / 2,
                CaptureRect.Height / 4),
            # 设置匹配的阈值
            Threshold = 0.7,
            # 设置是否在窗口上绘制识别区域
            DrawOnWindow = false
        }.InitTemplate();  # 初始化模板对象

        # 创建一个 RecognitionObject 实例，用于识别 LiftRodButton
        LiftRodButtonRo = new RecognitionObject
        {
            # 设置识别对象的名称
            Name = "LiftRodButton",
            # 设置识别类型为模板匹配
            RecognitionType = RecognitionTypes.TemplateMatch,
            # 加载并设置模板图像
            TemplateImageMat = GameTaskManager.LoadAssetImage("AutoFishing", "lift_rod.png"),
            # 设置识别区域的矩形范围
            RegionOfInterest = new Rect(CaptureRect.Width - CaptureRect.Width / 2,
                CaptureRect.Height - CaptureRect.Height / 4,
                CaptureRect.Width / 2,
                CaptureRect.Height / 4),
            # 设置匹配的阈值
            Threshold = 0.7,
            # 设置是否在窗口上绘制识别区域
            DrawOnWindow = false
        }.InitTemplate();  # 初始化模板对象

        # 计算退出钓鱼按钮的宽度和高度
        var w = (int)(140 * AssetScale);
        var h = (int)(150 * AssetScale);
        # 创建一个 RecognitionObject 实例，用于识别 ExitFishingButton
        ExitFishingButtonRo = new RecognitionObject
        {
            # 设置识别对象的名称
            Name = "ExitFishingButton",
            # 设置识别类型为模板匹配
            RecognitionType = RecognitionTypes.TemplateMatch,
            # 加载并设置模板图像
            TemplateImageMat = GameTaskManager.LoadAssetImage("AutoFishing", "exit_fishing.png"),
            # 设置识别区域的矩形范围
            RegionOfInterest = new Rect(CaptureRect.Width - w,
                CaptureRect.Height - h,
                w,
                h),
            # 设置匹配的阈值
            Threshold = 0.8,
            # 设置是否在窗口上绘制识别区域
            DrawOnWindow = false
        }.InitTemplate();  # 初始化模板对象
    }
# 这是代码块的结束大括号，用于标识代码块的结束
}
```