# `.\better-genshin-impact\BetterGenshinImpact\GameTask\AutoTrackPath\AutoTrackPathParam.cs`

```cs
# 导入 BetterGenshinImpact.GameTask.Model 命名空间中的内容
﻿using BetterGenshinImpact.GameTask.Model;
# 导入 System.Threading 命名空间中的内容
using System.Threading;

# 声明命名空间 BetterGenshinImpact.GameTask.AutoTrackPath
namespace BetterGenshinImpact.GameTask.AutoTrackPath;

# 定义 AutoTrackPathParam 类，继承自 BaseTaskParam 类
public class AutoTrackPathParam : BaseTaskParam
{
    # 定义构造函数，接受一个 CancellationTokenSource 对象作为参数
    public AutoTrackPathParam(CancellationTokenSource cts) : base(cts)
    {
    }
}
```