# `.\better-genshin-impact\BetterGenshinImpact\GameTask\QuickSereniteaPot\Assets\QuickSereniteaPotAssets.cs`

```cs
# 引用所需的命名空间和库
﻿using BetterGenshinImpact.Core.Recognition;
using BetterGenshinImpact.GameTask.Model;
using OpenCvSharp;

namespace BetterGenshinImpact.GameTask.QuickSereniteaPot.Assets;

# 定义 QuickSereniteaPotAssets 类，继承自 BaseAssets<QuickSereniteaPotAssets>
public class QuickSereniteaPotAssets : BaseAssets<QuickSereniteaPotAssets>
{
    # 声明两个 RecognitionObject 类型的公开字段，用于存储识别对象
    public RecognitionObject BagCloseButtonRo;
    public RecognitionObject SereniteaPotIconRo;

    # 定义私有构造函数，用于初始化 RecognitionObject 实例
    private QuickSereniteaPotAssets()
    {
        # 初始化 BagCloseButtonRo 对象，配置其识别属性
        BagCloseButtonRo = new RecognitionObject
        {
            Name = "MapCloseButton", # 设置识别对象的名称
            RecognitionType = RecognitionTypes.TemplateMatch, # 设置识别类型为模板匹配
            TemplateImageMat = GameTaskManager.LoadAssetImage("QuickTeleport", "MapCloseButton.png"), # 加载模板图像
            RegionOfInterest = new Rect(CaptureRect.Width - (int)(107 * AssetScale), # 设置兴趣区域的矩形，计算出位置
                (int)(19 * AssetScale),
                (int)(58 * AssetScale),
                (int)(58 * AssetScale)),
            DrawOnWindow = true # 设置是否在窗口上绘制识别区域
        }.InitTemplate(); # 初始化模板

        # 初始化 SereniteaPotIconRo 对象，配置其识别属性
        SereniteaPotIconRo = new RecognitionObject
        {
            Name = "SereniteaPotIcon", # 设置识别对象的名称
            RecognitionType = RecognitionTypes.TemplateMatch, # 设置识别类型为模板匹配
            TemplateImageMat = GameTaskManager.LoadAssetImage("QuickSereniteaPot", "SereniteaPotIcon.png"), # 加载模板图像
            RegionOfInterest = new Rect(
                (int)(100 * AssetScale), # 设置兴趣区域的矩形，计算出位置
                (int)(100 * AssetScale),
                (int)(1190 * AssetScale),
                (int)(860 * AssetScale)),
            DrawOnWindow = true # 设置是否在窗口上绘制识别区域
        }.InitTemplate(); # 初始化模板
    }
}
```