# `.\better-genshin-impact\BetterGenshinImpact\Core\Config\GenshinStartConfig.cs`

```cs
# 引入 CommunityToolkit.Mvvm.ComponentModel 命名空间，用于实现 MVVM 模式中的可观察对象
using CommunityToolkit.Mvvm.ComponentModel;
# 引入 System 命名空间，包含基本的系统功能
using System;

# 定义命名空间 BetterGenshinImpact.Core.Config，用于组织配置相关的类
namespace BetterGenshinImpact.Core.Config
{
    # 类描述：原神启动配置的模型类
    /// <summary>
    ///     原神启动配置
    /// </summary>
    # 标记该类为可序列化的，以便可以将其存储和传输
    [Serializable]
    # 定义一个部分类，允许类的定义分散在多个文件中
    public partial class GenshinStartConfig : ObservableObject
    {
        # 属性描述：自动点击月卡功能的启用状态
        /// <summary>
        ///     自动点击月卡
        /// </summary>
        # 标记该属性为可观察属性，以支持属性更改通知
        [ObservableProperty]
        # 定义一个私有字段来存储自动点击月卡功能的状态
        private bool _autoClickBlessingOfTheWelkinMoonEnabled;

        # 属性描述：自动进入游戏（开门）功能的启用状态
        /// <summary>
        ///     自动进入游戏（开门）
        /// </summary>
        # 标记该属性为可观察属性，以支持属性更改通知
        [ObservableProperty]
        # 定义一个私有字段来存储自动进入游戏功能的状态，默认值为 true
        private bool _autoEnterGameEnabled = true;

        # 属性描述：原神启动时的参数配置
        /// <summary>
        ///     原神启动参数
        /// </summary>
        # 标记该属性为可观察属性，以支持属性更改通知
        [ObservableProperty]
        # 定义一个私有字段来存储原神启动参数，默认值为空字符串
        private string _genshinStartArgs = "";

        # 属性描述：原神的安装路径
        /// <summary>
        ///     原神安装路径
        /// </summary>
        # 标记该属性为可观察属性，以支持属性更改通知
        [ObservableProperty]
        # 定义一个私有字段来存储原神的安装路径，默认值为空字符串
        private string _installPath = "";

        # 属性描述：联动启动原神本体的功能的启用状态
        /// <summary>
        ///     联动启动原神本体
        /// </summary>
        # 标记该属性为可观察属性，以支持属性更改通知
        [ObservableProperty]
        # 定义一个私有字段来存储联动启动功能的状态，默认值为 true
        private bool _linkedStartEnabled = true;
    }
}
```