# `.\better-genshin-impact\BetterGenshinImpact\GameTask\AutoGeniusInvokation\Config\DefaultTcgConfig.cs`

```cs
# 引入必要的命名空间
﻿using BetterGenshinImpact.Core.Config;
using BetterGenshinImpact.Service;
using System.Collections.Generic;
using System.IO;
using System.Linq;
using System.Text.Json;

namespace BetterGenshinImpact.GameTask.AutoGeniusInvokation.Config;

# 定义一个类 DefaultTcgConfig
public class DefaultTcgConfig
{

    # 定义一个静态属性，存储所有角色卡片的列表
    public static List<CharacterCard> CharacterCards { get; set; }
    # 定义一个静态属性，存储角色卡片的映射字典，以角色名称为键
    public static Dictionary<string, CharacterCard> CharacterCardMap { get; set; }

    # 静态构造函数，在类首次访问时执行
    static DefaultTcgConfig()
    {
        # 读取 JSON 文件的内容
        var json = File.ReadAllText(Global.Absolute(@"GameTask\AutoGeniusInvokation\Assets\tcg_character_card.json"));
        # 反序列化 JSON 字符串为 CharacterCard 对象的列表
        var config = JsonSerializer.Deserialize<List<CharacterCard>>(json, ConfigService.JsonOptions);
        # 如果反序列化失败则抛出异常，否则初始化 CharacterCards 和 CharacterCardMap
        CharacterCards = config ?? throw new System.Exception("tcg_character_card.json deserialize failed");
        # 使用角色卡片列表创建一个字典，键为角色名称，值为 CharacterCard 对象
        CharacterCardMap = config.ToDictionary(x => x.Name);
    }
}
```