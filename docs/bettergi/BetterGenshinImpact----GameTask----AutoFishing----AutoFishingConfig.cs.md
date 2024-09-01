# `.\better-genshin-impact\BetterGenshinImpact\GameTask\AutoFishing\AutoFishingConfig.cs`

```cs
﻿using CommunityToolkit.Mvvm.ComponentModel; // 引入 CommunityToolkit 的 MVVM 组件模型库
using System; // 引入系统基本功能库

namespace BetterGenshinImpact.GameTask.AutoFishing
{
    /// <summary>
    /// 自动钓鱼配置
    /// </summary>
    [Serializable] // 标记类为可序列化
    public partial class AutoFishingConfig : ObservableObject // 定义一个部分类，继承自 ObservableObject 用于数据绑定
    {
        /// <summary>
        /// 触发器是否启用
        /// 启用后：
        /// 1. 自动判断是否进入钓鱼状态
        /// 2. 自动提杆
        /// 3. 自动拉条
        /// </summary>
        [ObservableProperty] private bool _enabled = false; // 定义是否启用触发器的布尔属性，初始值为 false

        ///// <summary>
        ///// 鱼儿上钩文字识别区域
        ///// 暂时无用
        ///// </summary>
        //[ObservableProperty] private Rect _fishHookedRecognitionArea = Rect.Empty; // 注释掉的属性：鱼上钩文字识别区域，暂时未使用

        /// <summary>
        /// 自动抛竿是否启用
        /// </summary>
        [ObservableProperty] private bool _autoThrowRodEnabled = false; // 定义是否启用自动抛竿的布尔属性，初始值为 false

        /// <summary>
        /// 自动抛竿未上钩超时时间(秒)
        /// </summary>
        [ObservableProperty] private int _autoThrowRodTimeOut = 10; // 定义自动抛竿未上钩的超时时间（秒），初始值为 10
    }
}
```