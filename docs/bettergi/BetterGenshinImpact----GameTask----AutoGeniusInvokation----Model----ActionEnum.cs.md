# `.\better-genshin-impact\BetterGenshinImpact\GameTask\AutoGeniusInvokation\Model\ActionEnum.cs`

```cs
# 引入 System 命名空间
﻿using System;

# 定义 BetterGenshinImpact.GameTask.AutoGeniusInvokation.Model 命名空间
namespace BetterGenshinImpact.GameTask.AutoGeniusInvokation.Model
{
    # 定义一个枚举类型 ActionEnum，其中包含三个值
    public enum ActionEnum
    {
        ChooseFirst, SwitchLater, UseSkill
    }

    # 定义一个静态类 ActionEnumExtension，提供扩展方法
    public static class ActionEnumExtension
    {
        # 扩展字符串类型，提供将中文字符串转换为 ActionEnum 枚举值的方法
        public static ActionEnum ChineseToActionEnum(this string type)
        {
            # 将输入字符串转换为小写
            type = type.ToLower();
            # 根据字符串值匹配对应的 ActionEnum 枚举值
            return type switch
            {
                # 如果是 "出战"，抛出 ArgumentOutOfRangeException 异常
                "出战" => /* ActionEnum.ChooseFirst, */ throw new ArgumentOutOfRangeException(nameof(type), type, null),
                # 如果是 "切换"，抛出 ArgumentOutOfRangeException 异常
                "切换" => /* ActionEnum.SwitchLater, */ throw new ArgumentOutOfRangeException(nameof(type), type, null),
                # 如果是 "使用"，返回 ActionEnum.UseSkill
                "使用" => ActionEnum.UseSkill,
                # 如果字符串不匹配任何情况，抛出 ArgumentOutOfRangeException 异常
                _ => throw new ArgumentOutOfRangeException(nameof(type), type, null),
            };
        }

        # 扩展 ActionEnum 类型，提供将 ActionEnum 枚举值转换为中文字符串的方法
        public static string ToChinese(this ActionEnum type)
        {
            # 根据 ActionEnum 枚举值匹配对应的中文字符串
            return type switch
            {
                # 如果是 ActionEnum.ChooseFirst，返回 "出战"
                ActionEnum.ChooseFirst => "出战",
                # 如果是 ActionEnum.SwitchLater，返回 "切换"
                ActionEnum.SwitchLater => "切换",
                # 如果是 ActionEnum.UseSkill，返回 "使用"
                ActionEnum.UseSkill => "使用",
                # 如果枚举值不匹配任何情况，抛出 ArgumentOutOfRangeException 异常
                _ => throw new ArgumentOutOfRangeException(nameof(type), type, null),
            };
        }
    }
}
```