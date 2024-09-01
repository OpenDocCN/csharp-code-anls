# `.\better-genshin-impact\BetterGenshinImpact\GameTask\AutoWood\AutoWoodConfig.cs`

```cs
# 使用 CommunityToolkit.Mvvm.ComponentModel 命名空间
using CommunityToolkit.Mvvm.ComponentModel;
# 使用 System 命名空间
using System;

# 命名空间定义
namespace BetterGenshinImpact.GameTask.AutoWood
{
    # 类的描述：自动伐木配置
    /// <summary>
    /// 自动伐木配置
    /// </summary>
    # 类定义，继承 ObservableObject 以便于数据绑定和通知更改
    [Serializable]
    public partial class AutoWoodConfig : ObservableObject
    {
        # 属性描述：使用小道具后的额外延迟（毫秒）
        /// <summary>
        /// 使用小道具后的额外延迟（毫秒）
        /// </summary>
        # ObservableProperty 特性使该字段自动生成通知更改的代码
        [ObservableProperty]
        private int _afterZSleepDelay = 0;

        # 属性描述：木材数量OCR是否启用
        /// <summary>
        /// 木材数量OCR是否启用
        /// </summary>
        # ObservableProperty 特性使该字段自动生成通知更改的代码
        [ObservableProperty]
        private bool _woodCountOcrEnabled = false;

        # 下面的代码块被注释掉了，未启用的功能描述
        // /// <summary>
        // /// 按下两次ESC键，原因见：
        // /// https://github.com/babalae/better-genshin-impact/issues/235
        // /// </summary>
        // [ObservableProperty]
        // private bool _pressTwoEscEnabled = false;
    }
}
```