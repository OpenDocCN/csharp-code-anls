# `.\better-genshin-impact\BetterGenshinImpact\Core\Script\CancellationContext.cs`

```cs
# 引入 BetterGenshinImpact.Model 命名空间
﻿using BetterGenshinImpact.Model;
# 引入 System.Threading 命名空间
using System.Threading;

# 定义 BetterGenshinImpact.Core.Script 命名空间
namespace BetterGenshinImpact.Core.Script;

# 定义 CancellationContext 类，继承自 Singleton 类
public class CancellationContext : Singleton<CancellationContext>
{
    # 定义 Cts 属性，用于取消操作的令牌源
    public CancellationTokenSource Cts { get; set; } = new();

    # 标记对象是否已被处理
    private bool disposed;

    # 初始化 Cts 和 disposed
    public void Set()
    {
        Cts = new CancellationTokenSource();
        disposed = false;
    }

    # 取消操作，前提是对象未被处理
    public void Cancel()
    {
        if (!disposed)
        {
            Cts.Cancel();
        }
    }

    # 释放 Cts 资源并标记对象为已处理
    public void Clear()
    {
        Cts.Dispose();
        disposed = true;
    }
}
```