# `.\better-genshin-impact\BetterGenshinImpact\GameTask\Common\NewRetry.cs`

```cs
﻿using BetterGenshinImpact.GameTask.AutoGeniusInvokation.Exception; // 引入自定义的异常处理类
using System; // 引入系统命名空间
using System.Collections.Generic; // 引入泛型集合类命名空间
using System.Linq; // 引入 LINQ 查询功能
using System.Threading; // 引入线程操作功能

namespace BetterGenshinImpact.GameTask.Common; // 定义命名空间 BetterGenshinImpact.GameTask.Common

/// <summary>
/// https://stackoverflow.com/questions/1563191/cleanest-way-to-write-retry-logic
/// </summary>
public static class NewRetry // 定义一个静态类 NewRetry，用于实现重试逻辑
{
    public static void Do(Action action, TimeSpan retryInterval, int maxAttemptCount = 3) // 定义一个静态方法，接受一个无返回值的委托、重试间隔时间和最大重试次数
    {
        _ = Do<object?>(() => // 调用泛型重试方法，传入无返回值的委托和其他参数
        {
            action(); // 执行传入的操作
            return null; // 返回 null
        }, retryInterval, maxAttemptCount); // 传入重试间隔时间和最大重试次数
    }

    public static T Do<T>(Func<T> action, TimeSpan retryInterval, int maxAttemptCount = 3) // 定义一个泛型静态方法，接受一个有返回值的委托、重试间隔时间和最大重试次数
    {
        List<Exception> exceptions = []; // 初始化一个异常列表，用于记录发生的异常

        for (int attempted = 0; attempted < maxAttemptCount; attempted++) // 遍历重试次数
        {
            try
            {
                if (attempted > 0) // 如果不是第一次尝试
                {
                    Thread.Sleep(retryInterval); // 等待指定的重试间隔时间
                }

                return action(); // 执行传入的操作并返回其结果
            }
            catch (RetryException ex) // 捕获特定的重试异常
            {
                exceptions.Add(ex); // 将捕获的异常添加到异常列表中
            }
        }

        if (exceptions.Count > 0) // 如果异常列表中有异常
        {
            throw exceptions.Last(); // 抛出最后一个异常
        }
        throw new AggregateException(exceptions); // 否则，抛出包含所有异常的聚合异常
    }
}
```