# `.\better-genshin-impact\BetterGenshinImpact\Helpers\Extensions\BooleanExtension.cs`

```cs
# 定义一个命名空间 BetterGenshinImpact.Helpers.Extensions
﻿namespace BetterGenshinImpact.Helpers.Extensions;

# 定义一个静态类 BooleanExtension
public static class BooleanExtension
{
    # 定义一个扩展方法 ToChinese，用于将布尔值转换为中文表示
    public static string ToChinese(this bool enabled)
    {
        # 根据布尔值是否为 true 返回“开启”，否则返回“关闭”
        return enabled ? "开启" : "关闭";
    }
}
```