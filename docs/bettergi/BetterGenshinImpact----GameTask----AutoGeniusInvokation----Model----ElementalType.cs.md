# `.\better-genshin-impact\BetterGenshinImpact\GameTask\AutoGeniusInvokation\Model\ElementalType.cs`

```cs
# 定义一个命名空间，包含游戏任务和元素类型的扩展方法
namespace BetterGenshinImpact.GameTask.AutoGeniusInvokation.Model
{
    # 枚举类型，表示不同的元素类型
    public enum ElementalType
    {
        Omni,    # 代表所有元素
        Cryo,    # 冰元素
        Hydro,   # 水元素
        Pyro,    # 火元素
        Electro, # 雷元素
        Dendro,  # 草元素
        Anemo,   # 风元素
        Geo      # 岩元素
    }

    # 静态类，包含扩展方法用于转换元素类型
    public static class ElementalTypeExtension
    {
        # 扩展方法，将字符串转换为对应的 ElementalType
        public static ElementalType ToElementalType(this string type)
        {
            # 将字符串转换为小写
            type = type.ToLower();
            # 根据字符串值返回对应的 ElementalType
            return type switch
            {
                "omni" => ElementalType.Omni,         # "omni" 转换为 ElementalType.Omni
                "cryo" => ElementalType.Cryo,         # "cryo" 转换为 ElementalType.Cryo
                "hydro" => ElementalType.Hydro,       # "hydro" 转换为 ElementalType.Hydro
                "pyro" => ElementalType.Pyro,         # "pyro" 转换为 ElementalType.Pyro
                "electro" => ElementalType.Electro,   # "electro" 转换为 ElementalType.Electro
                "dendro" => ElementalType.Dendro,     # "dendro" 转换为 ElementalType.Dendro
                "anemo" => ElementalType.Anemo,       # "anemo" 转换为 ElementalType.Anemo
                "geo" => ElementalType.Geo,           # "geo" 转换为 ElementalType.Geo
                _ => throw new ArgumentOutOfRangeException(nameof(type), type, null), # 如果字符串不匹配任何值，抛出异常
            };
        }

        # 扩展方法，将中文字符串转换为对应的 ElementalType
        public static ElementalType ChineseToElementalType(this string type)
        {
            # 将字符串转换为小写
            type = type.ToLower();
            # 根据中文字符串值返回对应的 ElementalType
            return type switch
            {
                "全" => ElementalType.Omni,         # "全" 转换为 ElementalType.Omni
                "冰" => ElementalType.Cryo,         # "冰" 转换为 ElementalType.Cryo
                "水" => ElementalType.Hydro,        # "水" 转换为 ElementalType.Hydro
                "火" => ElementalType.Pyro,         # "火" 转换为 ElementalType.Pyro
                "雷" => ElementalType.Electro,      # "雷" 转换为 ElementalType.Electro
                "草" => ElementalType.Dendro,       # "草" 转换为 ElementalType.Dendro
                "风" => ElementalType.Anemo,        # "风" 转换为 ElementalType.Anemo
                "岩" => ElementalType.Geo,          # "岩" 转换为 ElementalType.Geo
                _ => throw new ArgumentOutOfRangeException(nameof(type), type, null), # 如果字符串不匹配任何值，抛出异常
            };
        }

        # 扩展方法，将 ElementalType 转换为对应的中文字符串
        public static string ToChinese(this ElementalType type)
        {
            # 根据 ElementalType 返回对应的中文字符串
            return type switch
            {
                ElementalType.Omni => "全",         # ElementalType.Omni 转换为 "全"
                ElementalType.Cryo => "冰",         # ElementalType.Cryo 转换为 "冰"
                ElementalType.Hydro => "水",        # ElementalType.Hydro 转换为 "水"
                ElementalType.Pyro => "火",         # ElementalType.Pyro 转换为 "火"
                ElementalType.Electro => "雷",      # ElementalType.Electro 转换为 "雷"
                ElementalType.Dendro => "草",       # ElementalType.Dendro 转换为 "草"
                ElementalType.Anemo => "风",        # ElementalType.Anemo 转换为 "风"
                ElementalType.Geo => "岩",          # ElementalType.Geo 转换为 "岩"
                _ => throw new ArgumentOutOfRangeException(nameof(type), type, null), # 如果 ElementalType 不匹配任何值，抛出异常
            };
        }

        # 扩展方法，将 ElementalType 转换为小写字符串
        public static string ToLowerString(this ElementalType type)
        {
            # 将 ElementalType 转换为字符串并转为小写
            return type.ToString().ToLower();
        }
    }
}
```