# `.\better-genshin-impact\BetterGenshinImpact\GameTask\AutoFight\Config\CombatAvatar.cs`

```cs
using System;  // 引入系统命名空间，提供基本功能
using System.Collections.Generic;  // 引入系统集合命名空间，支持集合类型

namespace BetterGenshinImpact.GameTask.AutoFight.Config;  // 定义命名空间，组织代码

[Serializable]  // 使类可以被序列化，以便存储或传输
public class CombatAvatar  // 定义一个公开类，表示战斗角色
{

    /// <summary>
    /// 唯一标识  // 描述 Id 属性的用途
    /// </summary>
    public string Id { get; set; } = string.Empty;  // 角色的唯一标识符，初始化为空字符串

    /// <summary>
    /// 角色中文名  // 描述 Name 属性的用途
    /// </summary>
    public string Name { get; set; } = string.Empty;  // 角色的中文名称，初始化为空字符串

    /// <summary>
    /// 角色英文名  // 描述 NameEn 属性的用途
    /// </summary>
    public string NameEn { get; set; } = string.Empty;  // 角色的英文名称，初始化为空字符串

    /// <summary>
    /// 武器类型  // 描述 Weapon 属性的用途
    /// </summary>
    public string Weapon { get; set; } = string.Empty;  // 角色的武器类型，初始化为空字符串

    /// <summary>
    /// 元素战技CD  // 描述 SkillCd 属性的用途
    /// </summary>
    public double SkillCd { get; set; }  // 元素战技的冷却时间，默认为 0

    /// <summary>
    /// 长按元素战技CD  // 描述 SkillHoldCd 属性的用途
    /// </summary>
    public double SkillHoldCd { get; set; }  // 长按元素战技的冷却时间，默认为 0

    /// <summary>
    /// 元素爆发CD  // 描述 BurstCd 属性的用途
    /// </summary>
    public double BurstCd { get; set; }  // 元素爆发的冷却时间，默认为 0

    /// <summary>
    /// 别名  // 描述 Alias 属性的用途
    /// </summary>
    public List<string> Alias { get; set; } = new List<string>();  // 角色的别名列表，初始化为空列表

}
```