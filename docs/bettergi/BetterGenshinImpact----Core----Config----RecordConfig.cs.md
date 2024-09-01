# `.\better-genshin-impact\BetterGenshinImpact\Core\Config\RecordConfig.cs`

```cs
﻿using CommunityToolkit.Mvvm.ComponentModel;  // 引入 CommunityToolkit.Mvvm.ComponentModel 命名空间，用于支持 MVVM 模式的功能
using System;  // 引入 System 命名空间，包含基本的系统功能

namespace BetterGenshinImpact.Core.Config;  // 定义 BetterGenshinImpact.Core.Config 命名空间

[Serializable]  // 表示这个类可以被序列化，以便保存和恢复其状态
public partial class RecordConfig : ObservableObject  // 定义一个公共的部分类 RecordConfig，继承自 ObservableObject，以便支持属性更改通知
{
    /// <summary>
    /// 视角每移动1度，需要MouseMoveBy的距离
    /// 用作脚本记录度数后转化的鼠标移动距离
    /// </summary>
    [ObservableProperty]  // 指示这个属性支持通知功能，当属性值发生变化时，会自动通知绑定的视图
    private double _angle2MouseMoveByX = 1.0;  // 定义一个私有字段，用于存储视角每移动1度需要的鼠标移动距离，初始化为 1.0

    /// <summary>
    /// 视角每移动1度，需要DirectInput移动的单位
    /// </summary>
    [ObservableProperty]  // 指示这个属性支持通知功能
    private double _angle2DirectInputX = 1.0;  // 定义一个私有字段，用于存储视角每移动1度需要的 DirectInput 移动单位，初始化为 1.0

    /// <summary>
    /// 图像识别记录相机视角朝向
    /// </summary>
    [ObservableProperty]  // 指示这个属性支持通知功能
    private bool _isRecordCameraOrientation = false;  // 定义一个私有字段，用于存储是否记录相机视角朝向，初始化为 false
}
```