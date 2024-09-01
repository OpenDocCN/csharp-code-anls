# `.\better-genshin-impact\BetterGenshinImpact\Core\Script\Dependence\GlobalMethod.cs`

```cs
# 引入异步编程所需的命名空间
﻿using System.Threading.Tasks;

# 定义一个命名空间，组织相关的代码
namespace BetterGenshinImpact.Core.Script.Dependence;

# 定义一个公共类，包含静态方法
public class GlobalMethod
{
    # 定义一个静态异步方法，用于延迟操作
    public static async Task Sleep(int millisecondsTimeout)
    {
        # 异步等待指定的时间，使用取消令牌进行取消控制
        await Task.Delay(millisecondsTimeout, CancellationContext.Instance.Cts.Token);
    }
}
```