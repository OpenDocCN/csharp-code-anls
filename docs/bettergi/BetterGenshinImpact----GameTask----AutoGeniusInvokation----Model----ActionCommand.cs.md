# `.\better-genshin-impact\BetterGenshinImpact\GameTask\AutoGeniusInvokation\Model\ActionCommand.cs`

```cs
﻿using System;

namespace BetterGenshinImpact.GameTask.AutoGeniusInvokation.Model
{
    public class ActionCommand
    {
        /// <summary>
        ///  角色
        /// </summary>
        // 角色属性，使用默认值初始化
        public Character Character { get; set; } = default!;

        // 行动枚举
        public ActionEnum Action { get; set; }

        /// <summary>
        /// 目标编号（技能编号，从右往左）
        /// </summary>
        // 目标索引，表示技能或角色的编号
        public int TargetIndex { get; set; }

        // 重写 ToString 方法以提供自定义字符串表示
        public override string? ToString()
        {
            // 如果行动是使用技能
            if (Action == ActionEnum.UseSkill)
            {
                // 如果技能名称为空
                if (string.IsNullOrEmpty(Character.Skills[TargetIndex].Name))
                {
                    // 返回默认技能编号的描述
                    return $"【{Character.Name}】使用【技能{TargetIndex}】";
                }
                else
                {
                    // 返回具体技能名称的描述
                    return $"【{Character.Name}】使用【{Character.Skills[TargetIndex].Name}】";
                }
            }
            // 如果行动是切换角色
            else if (Action == ActionEnum.SwitchLater)
            {
                // 返回切换角色的描述
                return $"【{Character.Name}】切换至【角色{TargetIndex}】";
            }
            else
            {
                // 默认的 ToString 实现
                return base.ToString();
            }
        }

        // 获取特定元素骰子的使用次数
        public int GetSpecificElementDiceUseCount()
        {
            // 如果行动是使用技能
            if (Action == ActionEnum.UseSkill)
            {
                // 返回技能的特定元素成本
                return Character.Skills[TargetIndex].SpecificElementCost;
            }
            else
            {
                // 如果行动未知，抛出异常
                throw new ArgumentException("未知行动");
            }
        }

        // 获取所有骰子的使用次数
        public int GetAllDiceUseCount()
        {
            // 如果行动是使用技能
            if (Action == ActionEnum.UseSkill)
            {
                // 返回技能的总成本
                return Character.Skills[TargetIndex].AllCost;
            }
            else
            {
                // 如果行动未知，抛出异常
                throw new ArgumentException("未知行动");
            }
        }

        // 获取使用骰子的元素类型
        public ElementalType GetDiceUseElementType()
        {
            // 如果行动是使用技能
            if (Action == ActionEnum.UseSkill)
            {
                // 返回角色的元素类型
                return Character.Element;
            }
            else
            {
                // 如果行动未知，抛出异常
                throw new ArgumentException("未知行动");
            }
        }

        // 切换角色的操作
        public bool SwitchLater()
        {
            // 调用角色的切换方法
            return Character.SwitchLater();
        }

        // 使用技能的操作
        public bool UseSkill(Duel duel)
        {
            // 调用角色的使用技能方法，传入对战对象
            return Character.UseSkill(TargetIndex, duel);
        }
    }
}
```