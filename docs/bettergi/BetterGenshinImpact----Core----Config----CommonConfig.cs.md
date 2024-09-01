# `.\better-genshin-impact\BetterGenshinImpact\Core\Config\CommonConfig.cs`

```cs
# 引入社区工具包 MVVM 组件模型的相关类
﻿using CommunityToolkit.Mvvm.ComponentModel;
# 引入系统命名空间
using System;

# 定义命名空间
namespace BetterGenshinImpact.Core.Config
{
    # 定义遮罩窗口配置的类
    /// <summary>
    ///     遮罩窗口配置
    /// </summary>
    [Serializable] # 使该类可被序列化
    public partial class CommonConfig : ObservableObject
    {
        # 定义是否启用遮罩窗口的属性
        /// <summary>
        ///     是否启用遮罩窗口
        /// </summary>
        [ObservableProperty] # 将该字段标记为可观察的属性
        private bool _screenshotEnabled; # 记录是否启用截图遮罩的状态

        # 定义 UID 遮盖是否启用的属性
        /// <summary>
        ///     UID遮盖是否启用
        /// </summary>
        [ObservableProperty] # 将该字段标记为可观察的属性
        private bool _screenshotUidCoverEnabled; # 记录 UID 遮盖是否启用的状态

        # 定义退出时是否最小化至托盘的属性
        /// <summary>
        ///     退出时最小化至托盘
        /// </summary>
        [ObservableProperty] # 将该字段标记为可观察的属性
        private bool _exitToTray; # 记录退出时是否最小化到托盘的状态
    }
}
```