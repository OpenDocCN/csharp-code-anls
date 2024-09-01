# `.\better-genshin-impact\BetterGenshinImpact\Helpers\AssertUtils.cs`

```cs
# 定义一个名为 BetterGenshinImpact.Helpers 的命名空间
﻿namespace BetterGenshinImpact.Helpers;

# 定义一个名为 AssertUtils 的公共类
public class AssertUtils
{
    # 定义一个公共静态方法 IsTrue，检查布尔值 b 是否为 true，不为 true 时抛出异常
    public static void IsTrue(bool b, string msg)
    {
        # 如果布尔值 b 为 false
        if (!b)
        {
            # 抛出异常，异常消息为提供的 msg
            throw new System.Exception(msg);
        }
    }
}
```