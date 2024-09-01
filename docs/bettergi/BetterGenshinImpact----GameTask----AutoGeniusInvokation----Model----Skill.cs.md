# `.\better-genshin-impact\BetterGenshinImpact\GameTask\AutoGeniusInvokation\Model\Skill.cs`

```cs
# 定义命名空间为 BetterGenshinImpact.GameTask.AutoGeniusInvokation.Model
﻿namespace BetterGenshinImpact.GameTask.AutoGeniusInvokation.Model;

# 定义 Skill 类
public class Skill
{
    # 对应游戏中技能的索引，1-4 和数组下标一致，游戏中是从右往左开始数的！
    public short Index { get; set; }

    # 技能的中文名称
    public string Name { get; set; } = string.Empty;

    # 技能的元素类型
    public ElementalType Type { get; set; }

    # 消耗指定元素骰子的数量
    public int SpecificElementCost { get; set; }

    # 消耗杂色骰子的数量，默认值为 0
    public int AnyElementCost { get; set; } = 0;

    # 指定元素骰子数量 + 杂色骰子数量 = 总骰子数量
    public int AllCost { get; set; }
}
```