# `.\better-genshin-impact\BetterGenshinImpact\GameTask\AutoFight\Script\CombatScript.cs`

```cs
# 引用 System.Collections.Generic 命名空间，用于集合类型的定义
﻿using System.Collections.Generic;

# 定义一个命名空间，包含有关自动战斗的脚本
namespace BetterGenshinImpact.GameTask.AutoFight.Script;

# 定义一个 CombatScript 类，构造函数接收两个参数
public class CombatScript(HashSet<string> avatarNames, List<CombatCommand> combatCommands)
{
    # 定义一个公开的属性 Name，初始化为空字符串
    public string Name { get; set; } = string.Empty;

    # 定义一个公开的属性 Path，初始化为空字符串
    public string Path { get; set; } = string.Empty;

    # 定义一个公开的属性 AvatarNames，初始化为传入的 avatarNames
    public HashSet<string> AvatarNames { get; set; } = avatarNames;

    # 定义一个公开的属性 CombatCommands，初始化为传入的 combatCommands
    public List<CombatCommand> CombatCommands { get; set; } = combatCommands;

    # 定义一个公开的属性 MatchCount，用于记录和队伍角色匹配到的数量，初始化为 0
    /// <summary>
    /// 用于记录和队伍角色匹配到的数量
    /// </summary>
    public int MatchCount { get; set; } = 0;
}
```