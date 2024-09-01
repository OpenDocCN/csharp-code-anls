# `.\better-genshin-impact\BetterGenshinImpact\GameTask\AutoPick\Assets\AutoPickAssets.cs`

```cs
# 引入必要的命名空间
﻿using BetterGenshinImpact.Core.Recognition;  # 引入识别相关功能
using BetterGenshinImpact.GameTask.Model;  # 引入游戏任务模型相关功能
using OpenCvSharp;  # 引入 OpenCVSharp 库，用于图像处理
using System.Drawing;  # 引入系统绘图功能

namespace BetterGenshinImpact.GameTask.AutoPick.Assets;  # 定义命名空间

# 定义自动拾取资源类，继承自 BaseAssets
public class AutoPickAssets : BaseAssets<AutoPickAssets>
{
    # 声明 RecognitionObject 类型的公有字段
    public RecognitionObject FRo;  # 识别对象 F
    public RecognitionObject ChatIconRo;  # 识别对象聊天图标
    public RecognitionObject SettingsIconRo;  # 识别对象设置图标

    # 私有构造函数，用于初始化识别对象
    private AutoPickAssets()
    {
        # 初始化 FRo 识别对象
        FRo = new RecognitionObject
        {
            Name = "F",  # 设置识别对象名称
            RecognitionType = RecognitionTypes.TemplateMatch,  # 设置识别类型为模板匹配
            TemplateImageMat = GameTaskManager.LoadAssetImage("AutoPick", "F.png"),  # 加载模板图像
            RegionOfInterest = new Rect((int)(1090 * AssetScale),  # 设置感兴趣区域的矩形区域
                (int)(330 * AssetScale),
                (int)(60 * AssetScale),
                (int)(420 * AssetScale)),
            DrawOnWindow = false  # 不在窗口上绘制
        }.InitTemplate();  # 初始化模板

        # 初始化 ChatIconRo 识别对象
        ChatIconRo = new RecognitionObject
        {
            Name = "ChatIcon",  # 设置识别对象名称
            RecognitionType = RecognitionTypes.TemplateMatch,  # 设置识别类型为模板匹配
            TemplateImageMat = GameTaskManager.LoadAssetImage("AutoSkip", "icon_option.png"),  # 加载模板图像
            DrawOnWindow = false,  # 不在窗口上绘制
            DrawOnWindowPen = new Pen(Color.Chocolate, 2)  # 设置绘制时使用的笔
        }.InitTemplate();  # 初始化模板

        # 初始化 SettingsIconRo 识别对象
        SettingsIconRo = new RecognitionObject
        {
            Name = "SettingsIcon",  # 设置识别对象名称
            RecognitionType = RecognitionTypes.TemplateMatch,  # 设置识别类型为模板匹配
            TemplateImageMat = GameTaskManager.LoadAssetImage("AutoPick", "icon_settings.png"),  # 加载模板图像
            DrawOnWindow = false,  # 不在窗口上绘制
            DrawOnWindowPen = new Pen(Color.Chocolate, 2)  # 设置绘制时使用的笔
        }.InitTemplate();  # 初始化模板
    }

    # 公共方法，加载自定义拾取键
    public RecognitionObject LoadCustomPickKey(string key)
    {
        return new RecognitionObject
        {
            Name = key,  # 设置识别对象名称为传入的键
            RecognitionType = RecognitionTypes.TemplateMatch,  # 设置识别类型为模板匹配
            TemplateImageMat = GameTaskManager.LoadAssetImage("AutoPick", key + ".png"),  # 加载模板图像
            RegionOfInterest = new Rect((int)(1090 * AssetScale),  # 设置感兴趣区域的矩形区域
                (int)(330 * AssetScale),
                (int)(60 * AssetScale),
                (int)(420 * AssetScale)),
            DrawOnWindow = false  # 不在窗口上绘制
        }.InitTemplate();  # 初始化模板
    }
}
```