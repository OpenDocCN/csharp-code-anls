# `.\better-genshin-impact\BetterGenshinImpact\GameTask\AutoFishing\Model\BaitType.cs`

```cs
# 引入需要的命名空间
﻿using System.Collections.Generic;

# 定义命名空间
namespace BetterGenshinImpact.GameTask.AutoFishing.Model;

# 定义 BaitType 类
public class BaitType
{
    # 定义不同的 BaitType 实例，使用 static readonly 保证只创建一次
    public static readonly BaitType FruitPasteBait = new("fruit paste bait", "果酿饵");
    public static readonly BaitType RedrotBait = new("redrot bait", "赤糜饵");
    public static readonly BaitType FalseWormBait = new("false worm bait", "蠕虫假饵");
    public static readonly BaitType FakeFlyBait = new("fake fly bait", "飞蝇假饵");
    public static readonly BaitType SugardewBait = new("sugardew bait", "甘露饵");
    public static readonly BaitType SourBait = new("sour bait", "酸桔饵");
    public static readonly BaitType FlashingMaintenanceMekBait = new("flashing maintenance mek bait", "维护机关频闪诱饵");

    # 定义一个只读属性，返回所有 BaitType 实例的集合
    public static IEnumerable<BaitType> Values
    {
        get
        {
            # 逐一返回所有 BaitType 实例
            yield return FruitPasteBait;
            yield return RedrotBait;
            yield return FalseWormBait;
            yield return FakeFlyBait;
            yield return SugardewBait;
            yield return SourBait;
            yield return FlashingMaintenanceMekBait;
        }
    }

    # 定义 BaitType 的两个属性
    public string Name { get; private set; }
    public string ChineseName { get; private set; }

    # 定义私有构造函数，用于初始化 BaitType 实例
    private BaitType(string name, string chineseName)
    {
        Name = name;
        ChineseName = chineseName;
    }

    # 根据名字返回对应的 BaitType 实例
    public static BaitType FromName(string name)
    {
        # 遍历所有 BaitType 实例
        foreach (var type in Values)
        {
            # 如果名字匹配，返回对应的实例
            if (type.Name == name)
            {
                return type;
            }
        }

        # 如果没有找到匹配的实例，抛出异常
        throw new KeyNotFoundException($"BaitType {name} not found");
    }
}
```