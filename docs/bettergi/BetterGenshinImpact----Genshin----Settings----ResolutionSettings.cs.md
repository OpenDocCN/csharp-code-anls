# `.\better-genshin-impact\BetterGenshinImpact\Genshin\Settings\ResolutionSettings.cs`

```cs
# 定义命名空间 BetterGenshinImpact.Genshin.Settings
namespace BetterGenshinImpact.Genshin.Settings;

# 定义 ResolutionSettings 类
public class ResolutionSettings
{
    # 受保护的字段，存储设置项的名称
    protected string? height_name = null;
    protected string? width_name = null;
    protected string? fullscreen_name = null;

    # 公共属性，存储屏幕高度
    public int Height { get; protected set; }
    # 公共属性，存储屏幕宽度
    public int Width { get; protected set; }
    # 公共属性，存储是否全屏
    public bool FullScreen { get; protected set; }

    # 构造函数，用于初始化 ResolutionSettings 实例
    public ResolutionSettings()
    {
        # 获取注册表中的键，如果获取失败则返回
        if (GenshinRegistry.GetRegistryKey() is not { } hk)
        {
            return;
        }

        # 获取注册表键中的所有值名称
        string[] names = hk.GetValueNames();

        # 遍历所有的值名称
        foreach (string name in names)
        {
            # 如果值名称包含 "Width"，设置 width_name
            if (name.Contains("Width"))
            {
                width_name = name;
            }
            # 如果值名称包含 "Height"，设置 height_name
            if (name.Contains("Height"))
            {
                height_name = name;
            }
            # 如果值名称包含 "Fullscreen"，设置 fullscreen_name
            if (name.Contains("Fullscreen"))
            {
                fullscreen_name = name;
            }
        }

        # 从注册表获取高度值并转换为整数
        Height = Convert.ToInt32(hk.GetValue(height_name));
        # 从注册表获取宽度值并转换为整数
        Width = Convert.ToInt32(hk.GetValue(width_name));
        # 从注册表获取全屏值并转换为布尔值（1 为真，0 为假）
        FullScreen = Convert.ToInt32(hk.GetValue(fullscreen_name)) == 1;
    }
}
```