# `.\better-genshin-impact\BetterGenshinImpact\GameTask\AutoGeniusInvokation\Model\RoundStrategy.cs`

```cs
# 引入所需的命名空间
﻿using System;
using System.Collections.Generic;

# 定义命名空间
namespace BetterGenshinImpact.GameTask.AutoGeniusInvokation.Model;

# 标记此类为过时，可能会在未来版本中移除
[Obsolete]
public class RoundStrategy
{
    # 定义一个公共属性，用于存储原始命令列表，初始化为空列表
    public List<string> RawCommandList { get; set; } = [];

    # 定义一个公共属性，用于存储动作命令列表，初始化为空列表
    public List<ActionCommand> ActionCommands { get; set; } = [];

    # 定义一个公共方法，根据当前回合的动作命令确定可能需要的元素类型
    public List<ElementalType> MaybeNeedElement(Duel duel)
    {
        # 初始化一个空列表，用于存储可能需要的元素类型
        List<ElementalType> result = [];

        # 遍历所有的动作命令
        for (int i = 0; i < ActionCommands.Count; i++)
        {
            # 检查当前动作是否是切换角色，并且后续动作是使用技能
            if (ActionCommands[i].Action == ActionEnum.SwitchLater
                && i != ActionCommands.Count - 1
                && ActionCommands[i + 1].Action == ActionEnum.UseSkill)
            {
                # 如果条件满足，则将目标角色的元素类型添加到结果列表中
                result.Add(duel.Characters[ActionCommands[i].TargetIndex].Element);
            }
        }
        # 返回可能需要的元素类型列表
        return result;
    }
}
```