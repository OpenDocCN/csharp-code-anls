# `.\better-genshin-impact\BetterGenshinImpact\GameTask\AutoMusicGame\AutoMusicGameParam.cs`

```cs
# 引用 BetterGenshinImpact.GameTask.Model 命名空间
﻿using BetterGenshinImpact.GameTask.Model;
# 引用 System.Threading 命名空间
using System.Threading;

# 定义命名空间 BetterGenshinImpact.GameTask.AutoMusicGame
namespace BetterGenshinImpact.GameTask.AutoMusicGame;

# 定义类 AutoMusicGameParam 继承自 BaseTaskParam，并接受一个 CancellationTokenSource 类型的参数
public class AutoMusicGameParam(CancellationTokenSource cts) : BaseTaskParam(cts);
```