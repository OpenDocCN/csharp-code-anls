# `.\better-genshin-impact\BetterGenshinImpact\GameTask\AutoFishing\Model\FishType.cs`

```cs
﻿using System;
using System.Collections.Generic;

namespace BetterGenshinImpact.GameTask.AutoFishing.Model;

/// <summary>
/// 模仿Java实现的多属性枚举类
/// </summary>
[Obsolete] // 标记此类为过时，不推荐使用
public class FishType
{
    // 定义静态字段 AizenMedaka 为 FishType 类型，并初始化其属性
    public static readonly FishType AizenMedaka = new("aizen medaka", "fruit paste bait", "蓝染花鳉");
    // 定义静态字段 Crystalfish 为 FishType 类型，并初始化其属性
    public static readonly FishType Crystalfish = new("crystalfish", "fruit paste bait", "水晶宴");
    // 定义静态字段 Dawncatcher 为 FishType 类型，并初始化其属性
    public static readonly FishType Dawncatcher = new("dawncatcher", "fruit paste bait", "擒霞客");
    // 定义静态字段 GlazeMedaka 为 FishType 类型，并初始化其属性
    public static readonly FishType GlazeMedaka = new("glaze medaka", "fruit paste bait", "琉璃花鳉");
    // 定义静态字段 Medaka 为 FishType 类型，并初始化其属性
    public static readonly FishType Medaka = new("medaka", "fruit paste bait", "花鳉");
    // 定义静态字段 SweetFlowerMedaka 为 FishType 类型，并初始化其属性
    public static readonly FishType SweetFlowerMedaka = new("sweet-flower medaka", "fruit paste bait", "甜甜花鳉");
    // 定义静态字段 AkaiMaou 为 FishType 类型，并初始化其属性
    public static readonly FishType AkaiMaou = new("akai maou", "redrot bait", "赤魔王");
    // 定义静态字段 Betta 为 FishType 类型，并初始化其属性
    public static readonly FishType Betta = new("betta", "redrot bait", "斗棘鱼");
    // 定义静态字段 LungedStickleback 为 FishType 类型，并初始化其属性
    public static readonly FishType LungedStickleback = new("lunged stickleback", "redrot bait", "肺棘鱼");
    // 定义静态字段 Snowstrider 为 FishType 类型，并初始化其属性
    public static readonly FishType Snowstrider = new("snowstrider", "redrot bait", "雪中君");
    // 定义静态字段 VenomspineFish 为 FishType 类型，并初始化其属性
    public static readonly FishType VenomspineFish = new("venomspine fish", "redrot bait", "鸩棘鱼");
    // 定义静态字段 AbidingAngelfish 为 FishType 类型，并初始化其属性
    public static readonly FishType AbidingAngelfish = new("abiding angelfish", "false worm bait", "长生仙");
    // 定义静态字段 BrownShirakodai 为 FishType 类型，并初始化其属性
    public static readonly FishType BrownShirakodai = new("brown shirakodai", "false worm bait", "流纹褐蝶鱼");
    // 定义静态字段 PurpleShirakodai 为 FishType 类型，并初始化其属性
    public static readonly FishType PurpleShirakodai = new("purple shirakodai", "false worm bait", "流纹京紫蝶鱼");
    // 定义静态字段 RaimeiAngelfish 为 FishType 类型，并初始化其属性
    public static readonly FishType RaimeiAngelfish = new("raimei angelfish", "false worm bait", "雷鸣仙");
    // 定义静态字段 TeaColoredShirakodai 为 FishType 类型，并初始化其属性
    public static readonly FishType TeaColoredShirakodai = new("tea-colored shirakodai", "false worm bait", "流纹茶蝶鱼");
    // 定义静态字段 BitterPufferfish 为 FishType 类型，并初始化其属性
    public static readonly FishType BitterPufferfish = new("bitter pufferfish", "fake fly bait", "苦炮鲀");
    // 定义静态字段 DivdaRay 为 FishType 类型，并初始化其属性
    public static readonly FishType DivdaRay = new("divda ray", "fake fly bait", "迪芙妲鳐");
    // 定义静态字段 FormaloRay 为 FishType 类型，并初始化其属性
    public static readonly FishType FormaloRay = new("formalo ray", "fake fly bait", "佛玛洛鳐");
    // 定义静态字段 GoldenKoi 为 FishType 类型，并初始化其属性
    public static readonly FishType GoldenKoi = new("golden koi", "fake fly bait", "金赤假龙");
    // 定义静态字段 Pufferfish 为 FishType 类型，并初始化其属性
    public static readonly FishType Pufferfish = new("pufferfish", "fake fly bait", "炮鲀");
    // 定义静态字段 RustyKoi 为 FishType 类型，并初始化其属性
    public static readonly FishType RustyKoi = new("rusty koi", "fake fly bait", "锖假龙");
    // 定义静态字段 HalcyonJadeAxeMarlin 为 FishType 类型，并初始化其属性
    public static readonly FishType HalcyonJadeAxeMarlin = new("halcyon jade axe marlin", "sugardew bait", "翡玉斧枪鱼");
    // 定义静态字段 LazuriteAxeMarlin 为 FishType 类型，并初始化其属性
    public static readonly FishType LazuriteAxeMarlin = new("lazurite axe marlin", "sugardew bait", "青金斧枪鱼");
    // 定义静态字段 PeachOfTheDeepWaves 为 FishType 类型，并初始化其属性
    public static readonly FishType PeachOfTheDeepWaves = new("peach of the deep waves", "sugardew bait", "沉波蜜桃");
    // 定义静态字段 SandstormAngler 为 FishType 类型，并初始化其属性
    public static readonly FishType SandstormAngler = new("sandstorm angler", "sugardew bait", "吹沙角鲀");
    // 定义静态字段 StreamingAxeMarlin 为 FishType 类型，并初始化其属性
    public static readonly FishType StreamingAxeMarlin = new("streaming axe marlin", "sugardew bait", "海涛斧枪鱼");
    # 定义一个静态的只读字段，表示一种名为“暮云角鲀”的鱼类型，使用的饵料是“sugardew bait”
    public static readonly FishType SunsetCloudAngler = new("sunset cloud angler", "sugardew bait", "暮云角鲀");
    # 定义一个静态的只读字段，表示一种名为“真果角鲀”的鱼类型，使用的饵料是“sugardew bait”
    public static readonly FishType TrueFruitAngler = new("true fruit angler", "sugardew bait", "真果角鲀");
    # 定义一个静态的只读字段，表示一种名为“烘烘心羽鲈”的鱼类型，使用的饵料是“sour bait”
    public static readonly FishType BlazingHeartfeatherBass = new("blazing heartfeather bass", "sour bait", "烘烘心羽鲈");
    # 定义一个静态的只读字段，表示一种名为“波波心羽鲈”的鱼类型，使用的饵料是“sour bait”
    public static readonly FishType RipplingHeartfeatherBass = new("rippling heartfeather bass", "sour bait", "波波心羽鲈");
    # 定义一个静态的只读字段，表示一种名为“维护机关·初始能力型”的鱼类型，使用的饵料是“flashing maintenance mek bait”
    public static readonly FishType MaintenanceMekInitialConfiguration = new("maintenance mek- initial configuration", "flashing maintenance mek bait", "维护机关·初始能力型");
    # 定义一个静态的只读字段，表示一种名为“维护机关·白金典藏型”的鱼类型，使用的饵料是“flashing maintenance mek bait”
    public static readonly FishType MaintenanceMekPlatinumCollection = new("maintenance mek- platinum collection", "flashing maintenance mek bait", "维护机关·白金典藏型");
    # 定义一个静态的只读字段，表示一种名为“维护机关·态势控制者”的鱼类型，使用的饵料是“flashing maintenance mek bait”
    public static readonly FishType MaintenanceMekSituationController = new("maintenance mek- situation controller", "flashing maintenance mek bait", "维护机关·态势控制者");
    # 定义一个静态的只读字段，表示一种名为“维护机关·水域清理者”的鱼类型，使用的饵料是“flashing maintenance mek bait”
    public static readonly FishType MaintenanceMekWaterBodyCleaner = new("maintenance mek- water body cleaner", "flashing maintenance mek bait", "维护机关·水域清理者");
    # 定义一个静态的只读字段，表示一种名为“维护机关·澄金领队型”的鱼类型，使用的饵料是“flashing maintenance mek bait”
    public static readonly FishType MaintenanceMekWaterGoldLeader = new("maintenance mek- gold leader", "flashing maintenance mek bait", "维护机关·澄金领队型");

    # 定义一个公开的只读属性，返回所有鱼类型的集合
    public static IEnumerable<FishType> Values
    {
        get
        {
            # 返回各个鱼类型的枚举值
            yield return AizenMedaka;
            yield return Crystalfish;
            yield return Dawncatcher;
            yield return GlazeMedaka;
            yield return Medaka;
            yield return SweetFlowerMedaka;
            yield return AkaiMaou;
            yield return Betta;
            yield return LungedStickleback;
            yield return Snowstrider;
            yield return VenomspineFish;
            yield return AbidingAngelfish;
            yield return BrownShirakodai;
            yield return PurpleShirakodai;
            yield return RaimeiAngelfish;
            yield return TeaColoredShirakodai;
            yield return BitterPufferfish;
            yield return DivdaRay;
            yield return FormaloRay;
            yield return GoldenKoi;
            yield return Pufferfish;
            yield return RustyKoi;
            yield return HalcyonJadeAxeMarlin;
            yield return LazuriteAxeMarlin;
            yield return PeachOfTheDeepWaves;
            yield return SandstormAngler;
            yield return StreamingAxeMarlin;
            yield return SunsetCloudAngler;
            yield return TrueFruitAngler;
            yield return BlazingHeartfeatherBass;
            yield return RipplingHeartfeatherBass;
            yield return MaintenanceMekInitialConfiguration;
            yield return MaintenanceMekPlatinumCollection;
            yield return MaintenanceMekSituationController;
            yield return MaintenanceMekWaterBodyCleaner;
            yield return MaintenanceMekWaterGoldLeader;
        }
    }

    # 定义一个公开的字符串属性，表示鱼的名称
    public string Name { get; private set; }
    # 公共属性，表示鱼的饵料名称，仅能被类内部设置
    public string BaitName { get; private set; }
    # 公共属性，表示鱼的中文名称，仅能被类内部设置
    public string ChineseName { get; private set; }

    # 私有构造函数，用于初始化 FishType 对象
    private FishType(string name, string baitName, string chineseName)
    {
        # 设置鱼的名称
        Name = name;
        # 设置鱼的饵料名称
        BaitName = baitName;
        # 设置鱼的中文名称
        ChineseName = chineseName;
    }

    # 公共静态方法，根据鱼的名称返回对应的 FishType 对象
    public static FishType FromName(string name)
    {
        # 遍历所有 FishType 对象
        foreach (var fishType in Values)
        {
            # 如果找到匹配的名称，则返回对应的 FishType 对象
            if (fishType.Name == name)
            {
                return fishType;
            }
        }

        # 如果没有找到匹配的名称，则抛出 KeyNotFoundException 异常
        throw new KeyNotFoundException($"FishType {name} not found");
    }
这段代码似乎不完整。您是否有其他代码或更具体的片段需要注释？
```