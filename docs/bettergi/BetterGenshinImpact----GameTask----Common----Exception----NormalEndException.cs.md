# `.\better-genshin-impact\BetterGenshinImpact\GameTask\Common\Exception\NormalEndException.cs`

```cs
# 定义命名空间，标明当前代码所在的逻辑分组
﻿namespace BetterGenshinImpact.GameTask.AutoGeniusInvokation.Exception; // TODO: change this namespace to BetterGenshinImpact.GameTask.Common.Exception

# 定义一个公共类 NormalEndException，继承自 System.Exception 类
# 该类用于表示特定的异常情况
public class NormalEndException(string message) : System.Exception(message)
{
}
```