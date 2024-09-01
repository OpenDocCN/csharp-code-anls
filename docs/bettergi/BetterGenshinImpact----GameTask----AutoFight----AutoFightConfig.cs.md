# `.\better-genshin-impact\BetterGenshinImpact\GameTask\AutoFight\AutoFightConfig.cs`

```cs
# 使用 CommunityToolkit.Mvvm.ComponentModel 命名空间的功能
using CommunityToolkit.Mvvm.ComponentModel;
# 引入 System 命名空间
using System;

# 声明命名空间 BetterGenshinImpact.GameTask.AutoFight
namespace BetterGenshinImpact.GameTask.AutoFight
{
    # <summary> 注释用于描述该类的用途
    # 自动战斗配置
    # </summary>
    # 标记该类为可序列化的
    [Serializable]
    # 声明 AutoFightConfig 类，继承自 ObservableObject
    public partial class AutoFightConfig : ObservableObject
    {
        # 为 _strategyName 字段生成通知属性
        [ObservableProperty] private string _strategyName = "";

        # <summary> 注释用于描述该属性的用途
        # 英文逗号分割 强制指定队伍角色
        # </summary>
        # 为 _teamNames 字段生成通知属性
        [ObservableProperty] private string _teamNames = "";
    }
}
```