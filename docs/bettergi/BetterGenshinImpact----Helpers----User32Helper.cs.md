# `.\better-genshin-impact\BetterGenshinImpact\Helpers\User32Helper.cs`

```cs
# 引入必要的命名空间和类
﻿using System;
using Vanara.PInvoke;

namespace BetterGenshinImpact.Helpers;

# 定义一个公共类 User32Helper
public class User32Helper
{
    # 定义一个公共静态方法 ToVk，将字符串键转换为 User32.VK 枚举值
    public static User32.VK ToVk(string key)
    {
        # 将输入的键转换为大写字母
        key = key.ToUpper();
        # 如果键的开头不是 "VK_"，则在键前添加 "VK_"
        if (!key.StartsWith("VK_"))
        {
            key = $"VK_{key}";
        }

        # 将键字符串转换为 User32.VK 枚举值并返回
        return (User32.VK)Enum.Parse(typeof(User32.VK), key);
    }
}
```