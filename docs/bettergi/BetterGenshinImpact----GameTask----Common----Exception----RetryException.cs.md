# `.\better-genshin-impact\BetterGenshinImpact\GameTask\Common\Exception\RetryException.cs`

```cs
# 定义命名空间，指定此类的逻辑分组位置
﻿namespace BetterGenshinImpact.GameTask.AutoGeniusInvokation.Exception; // TODO: change this namespace to BetterGenshinImpact.GameTask.Common.Exception

# 定义一个继承自 System.Exception 的自定义异常类
public class RetryException : System.Exception
{
    # 无参数构造函数，调用基类构造函数
    public RetryException() : base()
    {
    }

    # 带有消息参数的构造函数，调用基类构造函数
    public RetryException(string message) : base(message)
    {
    }
}
```