# `.\better-genshin-impact\BetterGenshinImpact\GameTask\QuickTeleport\Assets\QuickTeleportAssets.cs`

```cs
﻿using BetterGenshinImpact.Core.Recognition; // 引入识别相关库
using BetterGenshinImpact.GameTask.Model; // 引入游戏任务模型库
using OpenCvSharp; // 引入 OpenCV 相关库
using System.Collections.Generic; // 引入集合库

namespace BetterGenshinImpact.GameTask.QuickTeleport.Assets; // 定义命名空间

public class QuickTeleportAssets : BaseAssets<QuickTeleportAssets> // 定义 QuickTeleportAssets 类，继承自 BaseAssets
{
    public Rect MapChooseIconRoi; // 定义一个矩形区域用于选择地图图标
    public List<RecognitionObject> MapChooseIconRoList; // 定义地图选择图标的识别对象列表
    public List<Mat> MapChooseIconGreyMatList; // 定义地图选择图标的灰度图像矩阵列表

    public RecognitionObject TeleportButtonRo; // 定义传送按钮的识别对象
    public RecognitionObject MapScaleButtonRo; // 定义地图缩放按钮的识别对象
    public RecognitionObject MapCloseButtonRo; // 定义地图关闭按钮的识别对象
    public RecognitionObject MapChooseRo; // 定义地图选择的识别对象

    public RecognitionObject MapUndergroundSwitchButtonRo; // 定义地下切换按钮的识别对象
    public RecognitionObject MapUndergroundToGroundButtonRo; // 定义地下到地面按钮的识别对象

    private QuickTeleportAssets() // 定义私有构造函数
    }

    /// <summary>
    /// 宽泛的识别区域
    /// RegionOfInterest = new Rect(CaptureRect.Width / 2, CaptureRect.Height / 3,  CaptureRect.Width / 4, CaptureRect.Height - CaptureRect.Height / 3),
    ///
    /// 限制小区域，防止误识别
    /// </summary>
    /// <param name="name"></param>
    /// <returns></returns>
    public RecognitionObject BuildMapChooseIconRo(string name) // 定义构建地图选择图标识别对象的方法
    {
        var info = TaskContext.Instance().SystemInfo; // 获取系统信息
        var ro = new RecognitionObject // 创建识别对象实例
        {
            Name = name + "MapChooseIcon", // 设置识别对象名称
            RecognitionType = RecognitionTypes.TemplateMatch, // 设置识别类型为模板匹配
            TemplateImageMat = GameTaskManager.LoadAssetImage("QuickTeleport", name), // 加载模板图像
            RegionOfInterest = MapChooseIconRoi, // 设置感兴趣区域
            DrawOnWindow = false // 设置是否在窗口上绘制识别区域
        }.InitTemplate(); // 初始化模板

        if (name == "TeleportWaypoint.png") // 如果名称为 "TeleportWaypoint.png"
        {
            ro.Threshold = 0.7; // 设置识别阈值为 0.7
        }

        return ro; // 返回构建的识别对象
    }
}
```