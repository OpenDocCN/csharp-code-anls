# `.\better-genshin-impact\BetterGenshinImpact\GameTask\AutoFight\Model\AvatarMacro.cs`

```cs
# 引入需要的命名空间
﻿using BetterGenshinImpact.GameTask.AutoFight.Script;
using System.Collections.Generic;

# 定义命名空间
namespace BetterGenshinImpact.GameTask.AutoFight.Model;

# 定义 AvatarMacro 类
public class AvatarMacro
{
    # 定义 Name 属性，初始化为空字符串
    public string Name { get; set; } = string.Empty;
    # 定义 ScriptContent1 属性，初始化为空字符串
    public string ScriptContent1 { get; set; } = string.Empty;
    # 定义 ScriptContent2 属性，初始化为空字符串
    public string ScriptContent2 { get; set; } = string.Empty;
    # 定义 ScriptContent3 属性，初始化为空字符串
    public string ScriptContent3 { get; set; } = string.Empty;
    # 定义 ScriptContent4 属性，初始化为空字符串
    public string ScriptContent4 { get; set; } = string.Empty;
    # 定义 ScriptContent5 属性，初始化为空字符串
    public string ScriptContent5 { get; set; } = string.Empty;

    # 根据传入的索引返回相应的脚本内容
    public string GetScriptContent(int index)
    {
        return index switch
        {
            # 如果索引为 1，返回 ScriptContent1
            1 => ScriptContent1,
            # 如果索引为 2，返回 ScriptContent2
            2 => ScriptContent2,
            # 如果索引为 3，返回 ScriptContent3
            3 => ScriptContent3,
            # 如果索引为 4，返回 ScriptContent4
            4 => ScriptContent4,
            # 如果索引为 5，返回 ScriptContent5
            5 => ScriptContent5,
            # 如果索引不在以上范围，返回空字符串
            _ => string.Empty
        };
    }

    # 返回当前优先级的脚本内容
    public string GetScriptContent()
    {
        return GetScriptContent(TaskContext.Instance().Config.MacroConfig.CombatMacroPriority);
    }

    # 加载并解析命令，返回 CombatCommand 的列表
    public List<CombatCommand>? LoadCommands()
    {
        # 获取当前优先级的脚本内容
        var content = GetScriptContent();
        # 如果脚本内容为空或只包含空白字符，返回 null
        if (string.IsNullOrWhiteSpace(content))
        {
            return null;
        }
        # 解析脚本内容并返回 CombatCommand 列表
        return CombatScriptParser.ParseLineCommands(content, Name);
    }
}
```