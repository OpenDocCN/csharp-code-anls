# `.\better-genshin-impact\BetterGenshinImpact\GameTask\AutoGeniusInvokation\ScriptParser.cs`

```cs
# 引入 BetterGenshinImpact 游戏任务和配置相关的命名空间
﻿using BetterGenshinImpact.GameTask.AutoFight.Config;
using BetterGenshinImpact.GameTask.AutoGeniusInvokation.Config;
using BetterGenshinImpact.GameTask.AutoGeniusInvokation.Model;
using BetterGenshinImpact.Helpers;
using Microsoft.Extensions.Logging;
using System;
using System.Collections.Generic;
using System.Linq;
using System.Windows.Forms;

# 定义 BetterGenshinImpact.GameTask.AutoGeniusInvokation 命名空间
namespace BetterGenshinImpact.GameTask.AutoGeniusInvokation;

# 定义 ScriptParser 类
public class ScriptParser
{
    # 创建一个静态只读的 ILogger<ScriptParser> 实例，用于日志记录
    private static readonly ILogger<ScriptParser> MyLogger = App.GetLogger<ScriptParser>();

    # 公开静态方法 Parse，接受一个字符串形式的脚本，并返回一个 Duel 对象
    public static Duel Parse(string script)
    {
        # 按照行分隔符分割脚本字符串，生成字符串数组
        var lines = script.Split(["\r\n", "\r", "\n"], StringSplitOptions.None);
        # 创建一个字符串列表用于存储处理后的行
        var result = new List<string>();
        # 遍历每一行
        foreach (var line in lines)
        {
            # 去除行首尾的空白字符
            string l = line.Trim();
            # 将处理后的行添加到结果列表中
            result.Add(l);
        }

        # 调用另一个 Parse 方法处理结果列表，并返回处理后的 Duel 对象
        return Parse(result);
    }

    # 公开静态方法 Parse，接受一个字符串列表形式的脚本，并返回一个 Duel 对象
    public static Duel Parse(List<string> lines)
    # 创建一个新的 Duel 对象实例
    Duel duel = new();
    # 初始化 stage 变量为空字符串
    string stage = "";

    # 初始化行索引 i 为 0
    int i = 0;
    # 捕获可能的异常
    try
    {
        # 遍历所有行
        for (i = 0; i < lines.Count; i++)
        {
            # 获取当前行
            var line = lines[i];
            # 如果行包含冒号，则将其内容设置为当前 stage，并继续到下一行
            if (line.Contains(':'))
            {
                stage = line;
                continue;
            }

            # 如果行是分隔符、注释或空行，则跳过
            if (line == "---" || line.StartsWith("//") || string.IsNullOrEmpty(line))
            {
                continue;
            }

            # 如果 stage 是 "角色定义:"，则解析角色并添加到 Duel 对象中
            if (stage == "角色定义:")
            {
                var character = ParseCharacter(line);
                duel.Characters[character.Index] = character;
            }
            # 如果 stage 是 "策略定义:"，则解析策略
            else if (stage == "策略定义:")
            {
                # 确保第 3 个角色已定义
                MyAssert(duel.Characters[3] != null, "角色未定义");

                # 分割行中的行动部分
                string[] actionParts = line.Split(" ", StringSplitOptions.RemoveEmptyEntries);
                # 确保行动部分符合预期格式
                MyAssert(actionParts.Length == 3, "策略中的行动命令解析错误");
                MyAssert(actionParts[1] == "使用", "策略中的行动命令解析错误");

                # 创建新的 ActionCommand 实例并设置其 Action
                var actionCommand = new ActionCommand();
                var action = actionParts[1].ChineseToActionEnum();
                actionCommand.Action = action;

                # 找到匹配的角色并设置到 ActionCommand 中
                int j = 1;
                for (j = 1; j <= 3; j++)
                {
                    var character = duel.Characters[j];
                    if (character != null && character.Name == actionParts[0])
                    {
                        actionCommand.Character = character;
                        break;
                    }
                }

                # 确保角色匹配成功
                MyAssert(j <= 3, "策略中的行动命令解析错误：角色名称无法从角色定义中匹配到");

                # 解析技能编号并设置到 ActionCommand 中
                int skillNum = int.Parse(RegexHelper.ExcludeNumberRegex().Replace(actionParts[2], ""));
                MyAssert(skillNum < 5, "策略中的行动命令解析错误：技能编号错误");
                actionCommand.TargetIndex = skillNum;
                # 将 ActionCommand 添加到 Duel 对象的 ActionCommandQueue 中
                duel.ActionCommandQueue.Add(actionCommand);
            }
            # 如果 stage 不是已知的定义字段，抛出异常
            else
            {
                throw new System.Exception($"未知的定义字段：{stage}");
            }
        }

        # 确保第 3 个角色已定义
        MyAssert(duel.Characters[3] != null, "角色未定义，请确认策略文本格式是否为UTF-8");
    }
    # 捕获异常并记录错误信息
    catch (System.Exception ex)
    {
        MyLogger.LogError($"解析脚本错误，行号：{i + 1}，错误信息：{ex}");
        MessageBox.Error($"解析脚本错误，行号：{i + 1}，错误信息：{ex}", "策略解析失败");
        # 返回默认值
        return default!;
    }

    # 返回解析后的 Duel 对象
    return duel;
    {
        // 创建一个新的角色对象
        var character = new Character();

        // 将输入的行字符串按 '{' 分割，得到角色和技能部分
        var characterAndSkill = line.Split('{');

        // 将角色和技能部分按 '=' 分割，得到角色属性部分和技能字符串部分
        var parts = characterAndSkill[0].Split('=');
        // 从角色属性部分中提取角色序号，并将其转为整数
        character.Index = int.Parse(RegexHelper.ExcludeNumberRegex().Replace(parts[0], ""));
        // 断言角色序号必须在1到3之间
        MyAssert(character.Index >= 1 && character.Index <= 3, "角色序号必须在1-3之间");

        // 如果角色属性部分包含 '|'
        if (parts[1].Contains('|'))
        {
            // 将属性部分按 '|' 分割，得到角色名称和元素类型
            var nameAndElement = parts[1].Split('|');
            // 设置角色名称
            character.Name = nameAndElement[0];
            // 将元素字符转换为元素类型
            character.Element = nameAndElement[1][..1].ChineseToElementalType();

            // 处理技能部分
            string skillStr = characterAndSkill[1].Replace("}", "");
            // 将技能字符串按 ',' 分割，得到每个技能的字符串
            var skillParts = skillStr.Split(',');
            // 创建一个技能数组，长度比技能字符串数组多1
            var skills = new Skill[skillParts.Length + 1];
            // 遍历每个技能字符串
            for (int i = 0; i < skillParts.Length; i++)
            {
                // 解析技能字符串，得到技能对象
                var skill = ParseSkill(skillParts[i]);
                // 将技能添加到技能数组中
                skills[skill.Index] = skill;
            }

            // 设置角色的技能数组
            character.Skills = [.. skills];
        }
        else
        {
            // 如果不包含 '|', 使用默认配置
            character.Name = parts[1];
            // 获取默认名称，如果名称为空，则通过自动配置获取标准名称
            var standardName = DefaultTcgConfig.CharacterCardMap.Keys.FirstOrDefault(x => x.Equals(character.Name));
            if (string.IsNullOrEmpty(standardName))
            {
                standardName = DefaultAutoFightConfig.AvatarAliasToStandardName(character.Name);
            }

            // 如果存在标准名称对应的角色卡牌，则复制其属性到当前角色
            if (DefaultTcgConfig.CharacterCardMap.TryGetValue(standardName, out var characterCard))
            {
                CharacterCard.CopyCardProperty(character, characterCard);
            }
            else
            {
                // 如果没有对应的角色卡牌，则抛出异常
                throw new System.Exception($"角色【{standardName}】暂无默认卡牌定义配置，请自行进行角色定义");
            }
        }

        // 返回设置好的角色对象
        return character;
    }

    /// <summary>
    /// 技能3消耗=1雷骰子+2任意
    /// 技能2消耗=3雷骰子
    /// 技能1消耗=4雷骰子
    /// </summary>
    /// <param name="oneSkillStr">单个技能的描述字符串</param>
    /// <returns>解析后的技能对象</returns>
    public static Skill ParseSkill(string oneSkillStr)
    {
        // 创建一个新的技能对象
        var skill = new Skill();
        // 将技能字符串按 '=' 分割，得到技能序号部分和消耗部分
        var parts = oneSkillStr.Split('=');
        // 从技能序号部分中提取技能序号，并将其转为短整数
        skill.Index = short.Parse(RegexHelper.ExcludeNumberRegex().Replace(parts[0], ""));
        // 断言技能序号必须在1到5之间
        MyAssert(skill.Index >= 1 && skill.Index <= 5, "技能序号必须在1-5之间");
        // 获取技能的消耗字符串
        var costStr = parts[1];
        // 将消耗字符串按 '+' 分割，得到具体元素消耗和任意元素消耗部分
        var costParts = costStr.Split('+');
        // 设置具体元素消耗
        skill.SpecificElementCost = int.Parse(costParts[0][..1]);
        // 设置元素类型
        skill.Type = costParts[0].Substring(1, 1).ChineseToElementalType();
        // 如果存在 '+', 处理任意元素消耗
        if (costParts.Length > 1)
        {
            skill.AnyElementCost = int.Parse(costParts[1][..1]);
        }

        // 计算所有消耗的总和
        skill.AllCost = skill.SpecificElementCost + skill.AnyElementCost;
        // 返回解析后的技能对象
        return skill;
    }

    // 断言函数，用于检查条件是否为真，如果为假则抛出异常
    private static void MyAssert(bool b, string msg)
    {
        if (!b)
        {
            throw new System.Exception(msg);
        }
    }
你提供的代码是一个右花括号 `}`，似乎没有实际的内容。请确认一下代码内容或者提供更多上下文，以便我能为你添加适当的注释。
```