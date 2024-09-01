# `.\better-genshin-impact\BetterGenshinImpact\Genshin\Settings\GenshinRegistry.cs`

```cs
# 定义命名空间 BetterGenshinImpact.Genshin.Settings
using Microsoft.Win32;
using System;
using System.Diagnostics;

namespace BetterGenshinImpact.Genshin.Settings;

# 定义内部类 GenshinRegistry
internal class GenshinRegistry
{
    # 定义方法 GetRegistryKey，用于获取注册表键，参数为 GenshinRegistryType 类型，默认为 Auto
    public static RegistryKey? GetRegistryKey(GenshinRegistryType type = GenshinRegistryType.Auto)
    {
        try
        {
            # 获取当前用户的注册表根键
            using RegistryKey hkcu = Registry.CurrentUser;

            # 根据传入的类型决定查找的注册表子键
            if (type == GenshinRegistryType.Auto)
            {
                # 尝试打开中文名的注册表子键
                if (hkcu.OpenSubKey(@"SOFTWARE\miHoYo\原神", true) is RegistryKey sk)
                {
                    return sk;
                }
                # 尝试打开英文名的注册表子键
                if (hkcu.OpenSubKey(@"SOFTWARE\miHoYo\Genshin Impact", true) is RegistryKey sk)
                {
                    return sk;
                }
            }
            else if (type == GenshinRegistryType.Chinese)
            {
                # 如果类型为 Chinese，尝试打开中文名的注册表子键
                if (hkcu.OpenSubKey(@"SOFTWARE\miHoYo\原神", true) is RegistryKey sk)
                {
                    return sk;
                }
            }
            else if (type == GenshinRegistryType.Global)
            {
                # 如果类型为 Global，尝试打开英文名的注册表子键
                if (hkcu.OpenSubKey(@"SOFTWARE\miHoYo\Genshin Impact", true) is RegistryKey sk)
                {
                    return sk;
                }
            }
            else if (type == GenshinRegistryType.Cloud)
            {
                # 如果类型为 Cloud，抛出未实现异常
                throw new NotImplementedException();
            }
        }
        catch (Exception e)
        {
            # 捕获异常并写入调试输出
            Debug.WriteLine(e.ToString());
        }
        # 返回 null，表示未找到对应的注册表键
        return null;
    }
}

# 定义枚举 GenshinRegistryType，表示不同的注册表键类型
public enum GenshinRegistryType
{
    Auto,
    Chinese,
    Global,
    Cloud,
}
```