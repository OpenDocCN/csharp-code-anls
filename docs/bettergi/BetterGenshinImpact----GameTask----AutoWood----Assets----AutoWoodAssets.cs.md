# `.\better-genshin-impact\BetterGenshinImpact\GameTask\AutoWood\Assets\AutoWoodAssets.cs`

```cs
﻿using BetterGenshinImpact.Core.Recognition; // 引入用于图像识别的核心库
using BetterGenshinImpact.GameTask.Model; // 引入游戏任务模型库
using OpenCvSharp; // 引入 OpenCV 库用于计算机视觉功能

namespace BetterGenshinImpact.GameTask.AutoWood.Assets; // 定义命名空间

// 定义 AutoWoodAssets 类继承自 BaseAssets<AutoWoodAssets> 基类
public class AutoWoodAssets : BaseAssets<AutoWoodAssets>
{
    // 声明一个 RecognitionObject 类型的字段，表示“王树瑞佑”
    public RecognitionObject TheBoonOfTheElderTreeRo;

    // public RecognitionObject CharacterGuideRo; // 声明 CharacterGuideRo 字段，但目前被注释掉
    // 声明一个 RecognitionObject 类型的字段，表示菜单背包
    public RecognitionObject MenuBagRo;

    // 声明一个 RecognitionObject 类型的字段，表示确认按钮
    public RecognitionObject ConfirmRo;
    // 声明一个 RecognitionObject 类型的字段，表示进入游戏按钮
    public RecognitionObject EnterGameRo;

    // 声明一个 Rect 类型的字段，表示木头数量的矩形区域
    public Rect WoodCountUpperRect;

    // 构造函数，初始化 AutoWoodAssets 类的实例
    private AutoWoodAssets()
    {
        // 设置 WoodCountUpperRect 矩形区域的位置和大小
        WoodCountUpperRect = new Rect((int)(100 * AssetScale), (int)(450 * AssetScale), (int)(300 * AssetScale), (int)(250 * AssetScale));

        // 初始化 TheBoonOfTheElderTreeRo 对象，用于识别“王树瑞佑”
        TheBoonOfTheElderTreeRo = new RecognitionObject
        {
            Name = "TheBoonOfTheElderTree", // 设置对象名称
            RecognitionType = RecognitionTypes.TemplateMatch, // 设置识别类型为模板匹配
            TemplateImageMat = GameTaskManager.LoadAssetImage("AutoWood", "TheBoonOfTheElderTree.png"), // 加载模板图像
            RegionOfInterest = new Rect(CaptureRect.Width - CaptureRect.Width / 4, CaptureRect.Height / 2,
                CaptureRect.Width / 4, CaptureRect.Height - CaptureRect.Height / 2), // 设置感兴趣区域
            DrawOnWindow = false // 不在窗口上绘制识别区域
        }.InitTemplate(); // 初始化模板

        // CharacterGuideRo 的初始化代码被注释掉

        // 初始化 MenuBagRo 对象，用于识别菜单背包
        MenuBagRo = new RecognitionObject
        {
            Name = "MenuBag", // 设置对象名称
            RecognitionType = RecognitionTypes.TemplateMatch, // 设置识别类型为模板匹配
            TemplateImageMat = GameTaskManager.LoadAssetImage("AutoWood", "menu_bag.png"), // 加载模板图像
            RegionOfInterest = new Rect(0, 0, CaptureRect.Width / 2, CaptureRect.Height), // 设置感兴趣区域
            DrawOnWindow = false // 不在窗口上绘制识别区域
        }.InitTemplate(); // 初始化模板

        // 初始化 ConfirmRo 对象，用于识别确认按钮
        ConfirmRo = new RecognitionObject
        {
            Name = "AutoWoodConfirm", // 设置对象名称
            RecognitionType = RecognitionTypes.TemplateMatch, // 设置识别类型为模板匹配
            TemplateImageMat = GameTaskManager.LoadAssetImage("AutoWood", "confirm.png"), // 加载模板图像
            DrawOnWindow = false // 不在窗口上绘制识别区域
        }.InitTemplate(); // 初始化模板

        // 初始化 EnterGameRo 对象，用于识别进入游戏按钮
        EnterGameRo = new RecognitionObject
        {
            Name = "EnterGame", // 设置对象名称
            RecognitionType = RecognitionTypes.TemplateMatch, // 设置识别类型为模板匹配
            TemplateImageMat = GameTaskManager.LoadAssetImage("AutoWood", "exit_welcome.png"), // 加载模板图像
            RegionOfInterest = new Rect(0, CaptureRect.Height / 2, CaptureRect.Width, CaptureRect.Height - CaptureRect.Height / 2), // 设置感兴趣区域
            DrawOnWindow = false // 不在窗口上绘制识别区域
        }.InitTemplate(); // 初始化模板
    }
}
```