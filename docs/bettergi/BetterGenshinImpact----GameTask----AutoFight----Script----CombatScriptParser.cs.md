# `.\better-genshin-impact\BetterGenshinImpact\GameTask\AutoFight\Script\CombatScriptParser.cs`

```cs
# 命名空间声明
using BetterGenshinImpact.GameTask.AutoFight.Config;
using Microsoft.Extensions.Logging;
using System;
using System.Collections.Generic;
using System.IO;
using System.Linq;
using static BetterGenshinImpact.GameTask.Common.TaskControl;

namespace BetterGenshinImpact.GameTask.AutoFight.Script;

# CombatScriptParser 类定义
public class CombatScriptParser
{
    # 读取并解析战斗脚本
    public static CombatScriptBag ReadAndParse(string path)
    {
        # 如果路径是一个文件
        if (File.Exists(path))
        {
            # 解析文件并返回包含战斗脚本的袋子
            return new CombatScriptBag(Parse(path));
        }
        # 如果路径是一个目录
        else if (Directory.Exists(path))
        {
            # 获取目录下所有文本文件
            var files = Directory.GetFiles(path, "*.txt", SearchOption.AllDirectories);
            # 如果没有找到任何文件，记录错误并抛出异常
            if (files.Length == 0)
            {
                Logger.LogError("战斗脚本文件不存在：{Path}", path);
                throw new Exception("战斗脚本文件不存在");
            }

            # 初始化一个战斗脚本列表
            var combatScripts = new List<CombatScript>();
            # 遍历每个文件，尝试解析战斗脚本
            foreach (var file in files)
            {
                try
                {
                    combatScripts.Add(Parse(file));
                }
                catch (Exception e)
                {
                    # 解析失败时记录警告
                    Logger.LogWarning("解析战斗脚本文件失败：{Path} , {Msg} ", file, e.Message);
                }
            }

            # 返回包含所有解析脚本的袋子
            return new CombatScriptBag(combatScripts);
        }
        # 如果路径既不是文件也不是目录，记录错误并抛出异常
        else
        {
            Logger.LogError("战斗脚本文件不存在：{Path}", path);
            throw new Exception("战斗脚本文件不存在");
        }
    }

    # 解析单个战斗脚本文件
    public static CombatScript Parse(string path)
    {
        # 读取文件内容
        var script = File.ReadAllText(path);
        # 根据换行符和分号分割脚本内容
        var lines = script.Split(["\r\n", "\r", "\n", ";"], StringSplitOptions.RemoveEmptyEntries);
        # 初始化结果列表
        var result = new List<string>();
        # 遍历每行，进行格式化和清理
        foreach (var line in lines)
        {
            var l = line.Trim()
                .Replace("（", "(")
                .Replace(")", ")")
                .Replace("，", ",");
            # 跳过注释行和空行
            if (l.StartsWith("//") || l.StartsWith('#') || string.IsNullOrEmpty(l))
            {
                continue;
            }

            # 添加到结果列表
            result.Add(l);
        }

        # 解析结果行并创建战斗脚本对象
        var combatScript = Parse(result);
        # 设置战斗脚本的文件路径
        combatScript.Path = path;
        # 设置战斗脚本的名称（文件名不含扩展名）
        combatScript.Name = Path.GetFileNameWithoutExtension(path);
        return combatScript;
    }

    # 解析战斗脚本行
    public static CombatScript Parse(List<string> lines)
    {
        # 初始化战斗命令列表和角色名称集合
        List<CombatCommand> combatCommands = [];
        HashSet<string> combatAvatarNames = [];
        # 遍历每行，解析战斗命令
        foreach (var line in lines)
        {
            var oneLineCombatCommands = ParseLine(line, combatAvatarNames);
            combatCommands.AddRange(oneLineCombatCommands);
        }

        # 记录调试信息，包含命令数量和角色名称
        var names = string.Join(",", combatAvatarNames);
        Logger.LogDebug("战斗脚本解析完成，共{Cnt}条指令，涉及角色：{Str}", combatCommands.Count, names);

        # 返回包含角色和命令的战斗脚本对象
        return new CombatScript(combatAvatarNames, combatCommands);
    }

    # 解析战斗命令行
    public static List<CombatCommand> ParseLine(string line, HashSet<string> combatAvatarNames)
    {
        // 创建一个新的 CombatCommand 列表，用于存储解析出的指令
        var oneLineCombatCommands = new List<CombatCommand>();
        // 查找第一个空格的位置，以分隔角色名称和指令
        var firstSpaceIndex = line.IndexOf(' ');
        if (firstSpaceIndex < 0)
        {
            // 如果没有空格，日志错误并抛出异常
            Logger.LogError("战斗脚本格式错误，必须以空格分隔角色和指令");
            throw new Exception("战斗脚本格式错误，必须以空格分隔角色和指令");
        }

        // 提取角色名称
        var character = line[..firstSpaceIndex];
        // 将角色名称转换为标准名称
        character = DefaultAutoFightConfig.AvatarAliasToStandardName(character);
        // 提取指令部分
        var commands = line[(firstSpaceIndex + 1)..];
        // 解析指令，并将其添加到 oneLineCombatCommands 列表
        oneLineCombatCommands.AddRange(ParseLineCommands(commands, character));
        // 添加角色名称到 combatAvatarNames 列表
        combatAvatarNames.Add(character);
        // 返回解析后的指令列表
        return oneLineCombatCommands;
    }

    public static List<CombatCommand> ParseLineCommands(string lineWithoutAvatar, string avatarName)
    {
        // 创建一个新的 CombatCommand 列表，用于存储解析出的指令
        var oneLineCombatCommands = new List<CombatCommand>();
        // 将指令按逗号分隔成数组
        var commandArray = lineWithoutAvatar.Split(",", StringSplitOptions.RemoveEmptyEntries);

        for (var i = 0; i < commandArray.Length; i++)
        {
            // 获取当前指令
            var command = commandArray[i];
            if (string.IsNullOrEmpty(command))
            {
                // 如果指令为空，跳过
                continue;
            }

            // 检查指令中是否有开始括号但没有结束括号
            if (command.Contains('(') && !command.Contains(')'))
            {
                var j = i + 1;
                // 括号被逗号分隔，需要合并后续部分
                while (j < commandArray.Length)
                {
                    command += "," + commandArray[j];
                    // 检查括号配对是否正确
                    if (command.Count("(".Contains) > 1)
                    {
                        Logger.LogError("战斗脚本格式错误，指令 {Cmd} 括号无法配对", command);
                        throw new Exception("战斗脚本格式错误，指令括号无法配对");
                    }

                    if (command.Contains(')'))
                    {
                        i = j;
                        break;
                    }

                    j++;
                }

                // 检查括号是否完整
                if (!(command.Contains('(') && command.Contains(')')))
                {
                    Logger.LogError("战斗脚本格式错误，指令 {Cmd} 括号不完整", command);
                    throw new Exception("战斗脚本格式错误，指令括号不完整");
                }
            }

            // 创建 CombatCommand 对象并添加到列表
            var combatCommand = new CombatCommand(avatarName, command);
            oneLineCombatCommands.Add(combatCommand);
        }

        // 返回解析后的指令列表
        return oneLineCombatCommands;
    }
请提供需要注释的代码片段。
```