# `.\better-genshin-impact\BetterGenshinImpact\GameTask\QuickTeleport\QuickTeleportConfig.cs`

```cs
﻿using CommunityToolkit.Mvvm.ComponentModel; // 引入 CommunityToolkit 的 MVVM 组件模型库
using System; // 引入 System 命名空间

namespace BetterGenshinImpact.GameTask.QuickTeleport // 定义命名空间
{
    /// <summary>
    /// 快速传送配置 // 类的描述，表示这是一个快速传送的配置类
    /// </summary>
    [Serializable] // 表示该类可以被序列化
    public partial class QuickTeleportConfig : ObservableObject // 定义 QuickTeleportConfig 类，继承 ObservableObject
    {
        /// <summary>
        /// 快速传送是否启用 // 属性的描述，表示快速传送功能是否启用
        /// </summary>
        [ObservableProperty] private bool _enabled = false; // 定义一个私有字段 _enabled，并且自动生成相应的属性，初始值为 false

        /// <summary>
        /// 点击候选列表传送点的间隔时间(ms) // 属性的描述，表示点击传送点的间隔时间
        /// </summary>
        [ObservableProperty] private int _teleportListClickDelay = 200; // 定义一个私有字段 _teleportListClickDelay，并且自动生成相应的属性，初始值为 200 毫秒

        /// <summary>
        /// 等待右侧传送弹出界面的时间(ms) // 属性的描述，表示等待传送面板弹出的时间
        /// 0.24 版本后，这个值可以设置为 0，因为识图时间变久了。0.24 版本前，建议设置为 100 // 对应的版本变更说明
        /// </summary>
        [ObservableProperty] private int _waitTeleportPanelDelay = 50; // 定义一个私有字段 _waitTeleportPanelDelay，并且自动生成相应的属性，初始值为 50 毫秒

        /// <summary>
        /// 使用快捷键传送 // 属性的描述，表示是否启用快捷键传送
        /// </summary>
        [ObservableProperty] private bool _hotkeyTpEnabled = false; // 定义一个私有字段 _hotkeyTpEnabled，并且自动生成相应的属性，初始值为 false
    }
}
```