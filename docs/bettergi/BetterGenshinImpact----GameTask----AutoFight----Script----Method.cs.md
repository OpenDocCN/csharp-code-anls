# `.\better-genshin-impact\BetterGenshinImpact\GameTask\AutoFight\Script\Method.cs`

```cs
# 引入 Microsoft 扩展日志库
﻿using Microsoft.Extensions.Logging;
# 引入系统基础库
using System;
# 引入系统集合库
using System.Collections.Generic;
# 引入 TaskControl 静态类中的所有成员
using static BetterGenshinImpact.GameTask.Common.TaskControl;

# 定义命名空间
namespace BetterGenshinImpact.GameTask.AutoFight.Script;

# 定义 Method 类
public class Method
{
    # 定义 Skill 方法的静态常量，初始化时传入别名列表
    public static readonly Method Skill = new(["skill", "e"]);
    # 定义 Burst 方法的静态常量，初始化时传入别名列表
    public static readonly Method Burst = new(["burst", "q"]);
    # 定义 Attack 方法的静态常量，初始化时传入别名列表
    public static readonly Method Attack = new(["attack", "普攻", "普通攻击"]);
    # 定义 Charge 方法的静态常量，初始化时传入别名列表
    public static readonly Method Charge = new(["charge", "重击"]);
    # 定义 Wait 方法的静态常量，初始化时传入别名列表
    public static readonly Method Wait = new(["wait", "after", "等待"]);

    # 定义 Walk 方法的静态常量，初始化时传入别名列表
    public static readonly Method Walk = new(["walk", "行走"]);
    # 定义 W 方法的静态常量，初始化时传入别名列表
    public static readonly Method W = new(["w"]);
    # 定义 A 方法的静态常量，初始化时传入别名列表
    public static readonly Method A = new(["a"]);
    # 定义 S 方法的静态常量，初始化时传入别名列表
    public static readonly Method S = new(["s"]);
    # 定义 D 方法的静态常量，初始化时传入别名列表
    public static readonly Method D = new(["d"]);

    # 定义 Aim 方法的静态常量，初始化时传入别名列表
    public static readonly Method Aim = new(["aim", "r", "瞄准"]);
    # 定义 Dash 方法的静态常量，初始化时传入别名列表
    public static readonly Method Dash = new(["dash", "冲刺"]);
    # 定义 Jump 方法的静态常量，初始化时传入别名列表
    public static readonly Method Jump = new(["jump", "j", "跳跃"]);

    # 定义 MouseDown 方法的静态常量，初始化时传入别名列表
    public static readonly Method MouseDown = new(["mousedown"]);
    # 定义 MouseUp 方法的静态常量，初始化时传入别名列表
    public static readonly Method MouseUp = new(["mouseup"]);
    # 定义 Click 方法的静态常量，初始化时传入别名列表
    public static readonly Method Click = new(["click"]);
    # 定义 MoveBy 方法的静态常量，初始化时传入别名列表
    public static readonly Method MoveBy = new(["moveby"]);
    # 定义 KeyDown 方法的静态常量，初始化时传入别名列表
    public static readonly Method KeyDown = new(["keydown"]);
    # 定义 KeyUp 方法的静态常量，初始化时传入别名列表
    public static readonly Method KeyUp = new(["keyup"]);
    # 定义 KeyPress 方法的静态常量，初始化时传入别名列表
    public static readonly Method KeyPress = new(["keypress"]);

    # 定义一个只读属性，返回所有 Method 实例
    public static IEnumerable<Method> Values
    {
        get
        {
            # 逐个返回所有定义的 Method 实例
            yield return Skill;
            yield return Burst;
            yield return Attack;
            yield return Charge;
            yield return Wait;

            yield return Walk;
            yield return W;
            yield return A;
            yield return S;
            yield return D;

            # 取消注释将返回 Aim 方法
            # yield return Aim;
            yield return Dash;
            yield return Jump;

            # 返回所有宏定义的 Method 实例
            yield return MouseDown;
            yield return MouseUp;
            yield return Click;
            yield return MoveBy;
            yield return KeyDown;
            yield return KeyUp;
            yield return KeyPress;
        }
    }

    # 类成员：别名列表的属性
    /// <summary>
    /// 别名
    /// </summary>
    public List<string> Alias { get; private set; }

    # 构造函数：接收别名列表并初始化 Alias 属性
    public Method(List<string> alias)
    {
        Alias = alias;
    }

    # 根据方法名称获取对应的 Method 实例
    public static Method GetEnumByCode(string method)
    {
        # 遍历所有 Method 实例
        foreach (var m in Values)
        {
            # 如果别名列表包含指定的方法名称，返回对应的 Method 实例
            if (m.Alias.Contains(method))
            {
                return m;
            }
        }

        # 如果没有找到，记录错误日志并抛出异常
        Logger.LogError($"战斗策略脚本中出现未知的方法：{method}");
        throw new ArgumentException($"战斗策略脚本中出现未知的方法：{method}");
    }
}
```