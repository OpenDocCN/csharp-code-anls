# `.\better-genshin-impact\BetterGenshinImpact\Core\Script\Dependence\Model\TimerConfig\AutoPickExternalConfig.cs`

```cs
# 定义命名空间
﻿namespace BetterGenshinImpact.Core.Script.Dependence.Model.TimerConfig;

# 定义公共类 AutoPickExternalConfig
public class AutoPickExternalConfig
{
    # 公共属性，用于设置是否禁用黑白名单，默认为 false
    public bool DisabledBlackWhiteList { get; set; } = false;

    # 公共属性，存储需要 F 键操作的文本列表（对话、拾取），默认为空数组
    public string[] TextList { get; set; } = [];

    # 公共属性，设置是否无视文本和图标直接点击 F 键，默认为 false
    public bool ForceInteraction { get; set; } = false;
}
```