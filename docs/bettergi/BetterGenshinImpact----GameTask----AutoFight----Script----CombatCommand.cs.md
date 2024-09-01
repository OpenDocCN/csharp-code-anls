# `.\better-genshin-impact\BetterGenshinImpact\GameTask\AutoFight\Script\CombatCommand.cs`

```cs
# 引入所需的命名空间和类
﻿using BetterGenshinImpact.GameTask.AutoFight.Model;
using BetterGenshinImpact.Helpers;
using System;
using System.Collections.Generic;
using TimeSpan = System.TimeSpan;

namespace BetterGenshinImpact.GameTask.AutoFight.Script;

# 定义 CombatCommand 类
public class CombatCommand
{
    # 定义属性 Name，用于存储命令名称
    public string Name { get; set; }

    # 定义属性 Method，用于存储方法
    public Method Method { get; set; }

    # 定义属性 Args，用于存储参数列表
    public List<string>? Args { get; set; }

    # CombatCommand 类的构造函数，初始化命令名称和命令字符串
    public CombatCommand(string name, string command)
    {
        # 去除命令名称和命令字符串的多余空格
        Name = name.Trim();
        command = command.Trim();
        # 查找命令字符串中的开始括号索引
        var startIndex = command.IndexOf('(');
        # 如果找到开始括号
        if (startIndex > 0)
        {
            # 查找结束括号索引
            var endIndex = command.IndexOf(')');
            # 提取方法名称并去除多余空格
            var method = command[..startIndex];
            method = method.Trim();
            # 根据方法名称获取对应的枚举值
            Method = Method.GetEnumByCode(method);

            # 提取参数部分并按逗号分割成参数列表
            var parameters = command.Substring(startIndex + 1, endIndex - startIndex - 1);
            Args = new List<string>(parameters.Split(',', StringSplitOptions.TrimEntries));
            # 校验参数的正确性
            if (Method == Method.Walk)
            {
                # 验证 walk 方法的参数个数和参数值
                AssertUtils.IsTrue(Args.Count == 2, "walk方法必须有两个入参，第一个参数是方向，第二个参数是行走时间。例：walk(s, 0.2)");
                var s = double.Parse(Args[1]);
                AssertUtils.IsTrue(s > 0, "行走时间必须大于0");
            }
            else if (Method == Method.W || Method == Method.A || Method == Method.S || Method == Method.D)
            {
                # 验证 w/a/s/d 方法的参数个数
                AssertUtils.IsTrue(Args.Count == 1, "w/a/s/d方法必须有一个入参，代表行走时间。例：d(0.5)");
            }
            else if (Method == Method.MoveBy)
            {
                # 验证 moveby 方法的参数个数
                AssertUtils.IsTrue(Args.Count == 2, "moveby方法必须有两个入参，分别是x和y。例：moveby(100, 100))");
            }
            else if (Method == Method.KeyDown || Method == Method.KeyUp || Method == Method.KeyPress)
            {
                # 验证 KeyDown/KeyUp/KeyPress 方法的参数个数和合法性
                AssertUtils.IsTrue(Args.Count == 1, $"{Method.Alias[0]}方法必须有一个入参，代表按键");
                try
                {
                    User32Helper.ToVk(Args[0]);
                }
                catch
                {
                    throw new ArgumentException($"{Method.Alias[0]}方法的入参必须是VirtualKeyCodes枚举中的值，当前入参 {Args[0]} 不合法");
                }
            }
        }
        else
        {
            # 如果没有括号，则直接通过命令字符串获取方法枚举值
            Method = Method.GetEnumByCode(command);
        }
    }

    # 执行 CombatCommand
    public void Execute(CombatScenes combatScenes)
    {
        # 根据名称选择角色
        var avatar = combatScenes.SelectAvatar(Name);
        if (avatar == null)
        {
            return;
        }

        # 对于非宏类脚本，等待角色切换成功
        if (Method != Method.Wait
            && Method != Method.MouseDown
            && Method != Method.MouseUp
            && Method != Method.Click
            && Method != Method.MoveBy
            && Method != Method.KeyDown
            && Method != Method.KeyUp
            && Method != Method.KeyPress)
        {
            avatar.Switch();
        }

        # 执行具体的动作
        Execute(avatar);
    }

    # 另一个 Execute 方法的占位符（代码未完成）
    public void Execute(Avatar avatar)
    }
}
```