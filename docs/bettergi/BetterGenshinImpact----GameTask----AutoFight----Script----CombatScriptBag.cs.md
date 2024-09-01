# `.\better-genshin-impact\BetterGenshinImpact\GameTask\AutoFight\Script\CombatScriptBag.cs`

```cs
# 定义一个 CombatScriptBag 类，包含处理战斗脚本的逻辑
public class CombatScriptBag(List<CombatScript> combatScripts)
{
    # 初始化 CombatScripts 属性，赋值为传入的 combatScripts 列表
    private List<CombatScript> CombatScripts { get; set; } = combatScripts;

    # 定义一个构造函数，接收单个 CombatScript 对象，并将其封装成列表传给主构造函数
    public CombatScriptBag(CombatScript combatScript) : this([combatScript])
    {
    }

    # 定义一个方法，查找与给定 Avatar 数组匹配的战斗脚本
    public List<CombatCommand> FindCombatScript(Avatar[] avatars)
    {
        # 遍历 CombatScripts 列表中的每一个 CombatScript 对象
        foreach (var combatScript in CombatScripts)
        {
            # 初始化匹配计数器
            var matchCount = 0;
            # 遍历给定的 Avatar 数组
            foreach (var avatar in avatars)
            {
                # 如果 Avatar 的名字在当前 CombatScript 的 AvatarNames 列表中，则匹配计数加一
                if (combatScript.AvatarNames.Contains(avatar.Name))
                {
                    matchCount++;
                }

                # 如果匹配的数量等于 Avatar 数组的长度，则说明找到完整匹配的战斗脚本
                if (matchCount == avatars.Length)
                {
                    # 记录日志，表示找到了匹配的战斗脚本
                    Logger.LogInformation("匹配到战斗脚本：{Name}", combatScript.Name);
                    # 返回找到的战斗脚本的 CombatCommands 列表
                    return combatScript.CombatCommands;
                }
            }

            # 更新当前 CombatScript 的匹配计数
            combatScript.MatchCount = matchCount;
        }

        # 如果没有找到完整匹配的战斗脚本
        # 按照匹配计数进行降序排序，以便选择匹配度最高的战斗脚本
        CombatScripts.Sort((a, b) => b.MatchCount.CompareTo(a.MatchCount));
        # 如果匹配度最高的战斗脚本的匹配计数为零，则抛出异常，表示未匹配到任何战斗脚本
        if (CombatScripts[0].MatchCount == 0)
        {
            throw new Exception("未匹配到任何战斗脚本");
        }

        # 记录警告日志，表示未完整匹配到四人队伍，使用匹配度最高的战斗脚本
        Logger.LogWarning("未完整匹配到四人队伍，使用匹配度最高的队伍：{Name}", CombatScripts[0].Name);
        # 返回匹配度最高的战斗脚本的 CombatCommands 列表
        return CombatScripts[0].CombatCommands;
    }
}
```