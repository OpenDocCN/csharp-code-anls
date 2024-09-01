# `.\better-genshin-impact\BetterGenshinImpact\GameTask\AutoPick\AutoPickConfig.cs`

```cs
# 引入 MVVM 组件模型库
﻿using CommunityToolkit.Mvvm.ComponentModel;
# 引入系统命名空间
using System;

# 定义命名空间
namespace BetterGenshinImpact.GameTask.AutoPick
{
    # <summary>标签描述类的使用说明</summary>
    # 该类在非16:9分辨率下可能无法正常工作
    [Serializable]
    # 定义一个可序列化的部分类，继承自 ObservableObject
    public partial class AutoPickConfig : ObservableObject
    {
        # <summary>标签描述属性的使用说明</summary>
        # 触发器是否启用
        [ObservableProperty] private bool _enabled = true;

        # <summary>标签描述属性的使用说明</summary>
        # 1080p下拾取文字左边的起始偏移
        [ObservableProperty] private int _itemIconLeftOffset = 60;

        # <summary>标签描述属性的使用说明</summary>
        # 1080p下拾取文字的起始偏移
        [ObservableProperty] private int _itemTextLeftOffset = 115;

        # <summary>标签描述属性的使用说明</summary>
        # 1080p下拾取文字的终止偏移
        [ObservableProperty] private int _itemTextRightOffset = 400;

        # <summary>标签描述属性的使用说明</summary>
        # 文字识别引擎
        # - Paddle
        # - Yap
        [ObservableProperty]
        private string _ocrEngine = PickOcrEngineEnum.Paddle.ToString();

        # <summary>标签描述属性的使用说明</summary>
        # 急速模式
        # 无视文字识别结果，直接拾取
        [ObservableProperty] private bool _fastModeEnabled = false;

        # <summary>标签描述属性的使用说明</summary>
        # 自定义按键拾取
        [ObservableProperty] private string _pickKey = "F";
    }
}
```