# `.\better-genshin-impact\BetterGenshinImpact\Model\HotKeyTypeEnum.cs`

```cs
# 定义命名空间为 BetterGenshinImpact.Model
namespace BetterGenshinImpact.Model;

# 定义一个公共的枚举类型 HotKeyTypeEnum
public enum HotKeyTypeEnum
{
    GlobalRegister, // 全局热键
    KeyboardMonitor, // 键盘监听
}

# 定义一个静态类 HotKeyTypeEnumExtension，用于扩展 HotKeyTypeEnum 的功能
public static class HotKeyTypeEnumExtension
{
    # 定义一个扩展方法 ToChineseName，将 HotKeyTypeEnum 转换为中文名称
    public static string ToChineseName(this HotKeyTypeEnum type)
    {
        # 使用 switch 表达式根据类型返回对应的中文名称
        return type switch
        {
            HotKeyTypeEnum.GlobalRegister => "全局热键", // 当类型为 GlobalRegister 时返回对应中文
            HotKeyTypeEnum.KeyboardMonitor => "键鼠监听", // 当类型为 KeyboardMonitor 时返回对应中文
            _ => throw new ArgumentOutOfRangeException(nameof(type), type, null), // 处理异常情况，抛出参数超出范围的异常
        };
    }
}
```