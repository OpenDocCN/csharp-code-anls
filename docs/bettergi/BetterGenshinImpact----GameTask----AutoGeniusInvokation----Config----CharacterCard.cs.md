# `.\better-genshin-impact\BetterGenshinImpact\GameTask\AutoGeniusInvokation\Config\CharacterCard.cs`

```cs
# 引入命名空间
﻿using BetterGenshinImpact.GameTask.AutoGeniusInvokation.Model;
using BetterGenshinImpact.GameTask.Common;
using Microsoft.Extensions.Logging;
using System;
using System.Collections.Generic;

# 定义一个命名空间
namespace BetterGenshinImpact.GameTask.AutoGeniusInvokation.Config;

# 声明一个可序列化的类
[Serializable]
public class CostItem
{
    /// <summary>
    /// 唯一id
    /// </summary>
    # 定义一个整型属性，用于存储唯一标识符
    public int Id { get; set; }

    /// <summary>
    /// 类型名
    /// </summary>
    # 定义一个字符串属性，用于存储类型名称（英文）
    public string NameEn { get; set; } = string.Empty;

    /// <summary>
    /// unaligned_element    无色元素
    /// energy    充能
    /// </summary>
    # 定义一个字符串属性，用于存储类型（例如无色元素、充能）
    public string Type { get; set; } = string.Empty;

    /// <summary>
    /// 消耗多少
    /// </summary>
    # 定义一个整型属性，用于存储消耗量
    public int Count { get; set; }
}

# 声明一个可序列化的类
[Serializable]
public class SkillsItem
{
    /// <summary>
    /// 
    /// </summary>
    # 定义一个字符串属性，用于存储技能名称（英文）
    public string NameEn { get; set; } = string.Empty;

    /// <summary>
    /// 流天射术
    /// </summary>
    # 定义一个字符串属性，用于存储技能名称（本地化名称）
    public string Name { get; set; } = string.Empty;

    /// <summary>
    /// 
    /// </summary>
    # 定义一个字符串列表属性，用于存储技能标签
    public List<string> SkillTag { get; set; } = [];

    /// <summary>
    /// 
    /// </summary>
    # 定义一个 CostItem 对象的列表属性，用于存储技能的消耗项
    public List<CostItem> Cost { get; set; } = [];
}

# 声明一个可序列化的类
[Serializable]
public class CharacterCard
{
    /// <summary>
    /// 唯一id
    /// </summary>
    # 定义一个整型属性，用于存储唯一标识符
    public int Id { get; set; }

    /// <summary>
    /// 
    /// </summary>
    # 定义一个字符串属性，用于存储角色名称（英文）
    public string NameEn { get; set; } = string.Empty;

    /// <summary>
    /// 
    /// </summary>
    # 定义一个字符串属性，用于存储角色类型
    public string Type { get; set; } = string.Empty;

    /// <summary>
    /// 甘雨
    /// </summary>
    # 定义一个字符串属性，用于存储角色名称（本地化名称）
    public string Name { get; set; } = string.Empty;

    /// <summary>
    /// 
    /// </summary>
    # 定义一个整型属性，用于存储角色的生命值
    public int Hp { get; set; }

    /// <summary>
    /// 
    /// </summary>
    # 定义一个整型属性，用于存储角色的能量值
    public int Energy { get; set; }

    /// <summary>
    /// 冰元素
    /// </summary>
    # 定义一个字符串属性，用于存储角色的元素属性（如冰元素）
    public string Element { get; set; } = string.Empty;

    /// <summary>
    /// 弓
    /// </summary>
    # 定义一个字符串属性，用于存储角色的武器类型（如弓）
    public string Weapon { get; set; } = string.Empty;

    /// <summary>
    /// 
    /// </summary>
    # 定义一个 SkillsItem 对象的列表属性，用于存储角色的技能
    public List<SkillsItem> Skills { get; set; } = [];

    # 静态方法，用于复制角色属性
    public static void CopyCardProperty(Character source, CharacterCard characterCard)
    {
        try
        {
            # 从角色卡片中提取并转换元素信息
            source.Element = characterCard.Element.Replace("元素", "").ChineseToElementalType();
            # 复制角色的 HP 值到目标对象
            source.Hp = characterCard.Hp;
            # 初始化技能数组，长度比角色卡片中的技能数量多一个
            source.Skills = new Skill[characterCard.Skills.Count + 1];

            # 技能数组的索引
            short skillIndex = 0;
            # 从角色卡片的技能列表中倒序遍历技能
            for (var i = characterCard.Skills.Count - 1; i >= 0; i--)
            {
                # 当前技能项
                var skillsItem = characterCard.Skills[i];
                # 如果技能标签包含“被动技能”，则跳过该技能
                if (skillsItem.SkillTag.Contains("被动技能"))
                {
                    continue;
                }

                # 增加技能索引
                skillIndex++;

                # 从技能项获取技能并设置索引
                source.Skills[skillIndex] = GetSkill(skillsItem);
                source.Skills[skillIndex].Index = skillIndex;
            }
        }
        catch (System.Exception e)
        {
            # 记录角色卡牌配置解析失败的错误日志
            TaskControl.Logger.LogError($"角色【{characterCard.Name}】卡牌配置解析失败：{e.Message}");
            # 抛出新的异常，提示角色定义问题
            throw new System.Exception($"角色【{characterCard.Name}】卡牌配置解析失败：{e.Message}。请自行进行角色定义", e);
        }
    }

    public static Skill GetSkill(SkillsItem skillsItem)
    {
        # 创建一个新的技能对象并初始化名称
        Skill skill = new()
        {
            Name = skillsItem.Name
        };
        # 记录具体元素成本的数量
        var specificElementNum = 0;
        # 遍历技能项中的所有成本
        foreach (var cost in skillsItem.Cost)
        {
            # 如果成本是“unaligned_element”，设置任意元素成本
            if (cost.NameEn == "unaligned_element")
            {
                skill.AnyElementCost = cost.Count;
            }
            # 如果成本是“energy”，则跳过
            else if (cost.NameEn == "energy")
            {
                continue;
            }
            # 其他成本设置为具体元素成本，并记录元素类型
            else
            {
                skill.SpecificElementCost = cost.Count;
                skill.Type = cost.NameEn.ToElementalType();
                specificElementNum++;
            }
        }

        # 如果具体元素成本数量不等于 1，则抛出异常
        if (specificElementNum != 1)
        {
            throw new System.Exception($"技能[{skillsItem.Name}]默认技能数据技能解析失败");
        }

        # 计算技能的所有成本
        skill.AllCost = skill.SpecificElementCost + skill.AnyElementCost;
        # 返回技能对象
        return skill;
    }
# 结束一个代码块或语句
}
```