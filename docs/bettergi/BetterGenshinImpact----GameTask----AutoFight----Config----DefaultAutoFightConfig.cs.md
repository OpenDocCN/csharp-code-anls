# `.\better-genshin-impact\BetterGenshinImpact\GameTask\AutoFight\Config\DefaultAutoFightConfig.cs`

```cs
# 引入所需的命名空间
﻿using BetterGenshinImpact.Core.Config;
using BetterGenshinImpact.Service;
using System;
using System.Collections.Generic;
using System.IO;
using System.Linq;
using System.Text.Json;

namespace BetterGenshinImpact.GameTask.AutoFight.Config;

# 定义一个类 DefaultAutoFightConfig
public class DefaultAutoFightConfig
{
    # 定义静态属性 CombatAvatars，存储 CombatAvatar 对象的列表
    public static List<CombatAvatar> CombatAvatars { get; set; }
    # 定义静态属性 CombatAvatarNames，存储角色名字的列表
    public static List<string> CombatAvatarNames { get; set; }
    # 定义静态属性 CombatAvatarMap，以角色名字为键，CombatAvatar 对象为值的字典
    public static Dictionary<string, CombatAvatar> CombatAvatarMap { get; set; }
    # 定义静态属性 CombatAvatarNameEnMap，以英文角色名字为键，CombatAvatar 对象为值的字典
    public static Dictionary<string, CombatAvatar> CombatAvatarNameEnMap { get; set; }

    # 静态构造函数，用于初始化静态属性
    static DefaultAutoFightConfig()
    {
        # 读取指定路径的 JSON 文件内容
        var json = File.ReadAllText(Global.Absolute(@"GameTask\AutoFight\Assets\combat_avatar.json"));
        # 将 JSON 字符串解析为 CombatAvatar 对象的列表
        var config = JsonSerializer.Deserialize<List<CombatAvatar>>(json, ConfigService.JsonOptions);
        # 如果解析结果为空，则抛出异常
        CombatAvatars = config ?? throw new Exception("combat_avatar.json deserialize failed");
        # 从解析结果中提取角色名字，存储到 CombatAvatarNames 列表
        CombatAvatarNames = config.Select(x => x.Name).ToList();
        # 根据角色名字创建 CombatAvatarMap 字典
        CombatAvatarMap = config.ToDictionary(x => x.Name);
        # 根据英文角色名字创建 CombatAvatarNameEnMap 字典
        CombatAvatarNameEnMap = config.ToDictionary(x => x.NameEn);
    }

    # 根据别名获取标准角色名字
    public static string AvatarAliasToStandardName(string alias)
    {
        # 查找别名包含指定字符串的 CombatAvatar 对象，如果未找到，则抛出异常
        var avatar = CombatAvatars.Find(x => x.Alias.Contains(alias)) ?? throw new Exception($"角色名称校验失败：{alias}");
        # 返回角色的标准名字
        return avatar.Name;
    }
}
```