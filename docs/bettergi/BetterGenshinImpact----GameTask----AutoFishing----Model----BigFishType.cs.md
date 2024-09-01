# `.\better-genshin-impact\BetterGenshinImpact\GameTask\AutoFishing\Model\BigFishType.cs`

```cs
# 命名空间定义
namespace BetterGenshinImpact.GameTask.AutoFishing.Model
{
    /// <summary>
    /// 模仿Java实现的多属性枚举类
    /// 按形态大类分类的原神鱼类枚举
    /// </summary>
    public class BigFishType
    {
        # 定义静态只读字段，表示不同的鱼类
        public static readonly BigFishType Medaka = new("medaka", "fruit paste bait", "花鳉");
        public static readonly BigFishType LargeMedaka = new("large medaka", "fruit paste bait", "大花鳉");
        public static readonly BigFishType Stickleback = new("stickleback", "redrot bait", "棘鱼");
        public static readonly BigFishType Koi = new("koi", "fake fly bait", "假龙");
        public static readonly BigFishType Butterflyfish = new("butterflyfish", "false worm bait", "蝶鱼");
        public static readonly BigFishType Pufferfish = new("pufferfish", "fake fly bait", "炮鲀");
        public static readonly BigFishType FormaloRay = new("formalo ray", "fake fly bait", "佛玛洛鳐");
        public static readonly BigFishType DivdaRay = new("divda ray", "fake fly bait", "迪芙妲鳐");
        public static readonly BigFishType Angler = new("angler", "sugardew bait", "角鲀");
        public static readonly BigFishType AxeMarlin = new("axe marlin", "sugardew bait", "斧枪鱼");
        public static readonly BigFishType HeartfeatherBass = new("heartfeather bass", "sour bait", "心羽鲈");
        public static readonly BigFishType MaintenanceMek = new("maintenance mek", "flashing maintenance mek bait", "维护机关");

        # 获取所有鱼类类型的集合
        public static IEnumerable<BigFishType> Values
        {
            get
            {
                yield return Medaka;
                yield return LargeMedaka;
                yield return Stickleback;
                yield return Koi;
                yield return Butterflyfish;
                yield return Pufferfish;
                yield return FormaloRay;
                yield return DivdaRay;
                yield return Angler;
                yield return AxeMarlin;
                yield return HeartfeatherBass;
                yield return MaintenanceMek;
            }
        }

        # 鱼类名称
        public string Name { get; private set; }
        # 鱼类使用的饵料名称
        public string BaitName { get; private set; }
        # 鱼类的中文名称
        public string ChineseName { get; private set; }

        # 私有构造函数，用于初始化鱼类类型
        private BigFishType(string name, string baitName, string chineseName)
        {
            Name = name;
            BaitName = baitName;
            ChineseName = chineseName;
        }

        # 根据名称返回对应的鱼类类型
        public static BigFishType FromName(string name)
        {
            foreach (var fishType in Values)
            {
                if (fishType.Name == name)
                {
                    return fishType;
                }
            }

            throw new KeyNotFoundException($"BigFishType {name} not found");
        }

        # 获取鱼类类型在集合中的索引
        public static int GetIndex(BigFishType e)
        {
            for (int i = 0; i < Values.Count(); i++)
            {
                if (Values.ElementAt(i).Name == e.Name)
                {
                    return i;
                }
            }
            throw new KeyNotFoundException($"BigFishType {e.Name} not found index");
        }
    }
}
```