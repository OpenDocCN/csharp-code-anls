# `.\better-genshin-impact\BetterGenshinImpact\GameTask\AutoCook\AutoPickConfig.cs`

```cs
# 引入 CommunityToolkit.Mvvm.ComponentModel 命名空间
﻿using CommunityToolkit.Mvvm.ComponentModel;
# 引入 System 命名空间
using System;

# 定义命名空间 BetterGenshinImpact.GameTask.AutoCook
namespace BetterGenshinImpact.GameTask.AutoCook
{
    # 定义一个类 AutoCookConfig，用于自动烹饪配置，标记为可序列化
    /// <summary>
    /// 自动烹饪配置
    /// </summary>
    [Serializable]
    public partial class AutoCookConfig : ObservableObject
    {
        # 定义一个属性 _enabled，表示触发器是否启用，继承自 ObservableObject
        /// <summary>
        /// 触发器是否启用
        /// </summary>
        [ObservableProperty]
        private bool _enabled = false;
    }
}
```