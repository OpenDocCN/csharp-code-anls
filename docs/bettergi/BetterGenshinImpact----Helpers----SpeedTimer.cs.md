# `.\better-genshin-impact\BetterGenshinImpact\Helpers\SpeedTimer.cs`

```cs
# 引入必要的命名空间
﻿using System;
using System.Collections.Generic;
using System.Diagnostics;

namespace BetterGenshinImpact.Helpers;

# 定义 SpeedTimer 类
public class SpeedTimer
{
    # 定义用于计时的 Stopwatch 对象
    private readonly Stopwatch _stopwatch;

    # 定义存储时间记录的字典
    private readonly Dictionary<string, TimeSpan> _timeRecordDic = [];

    # 构造函数，初始化 Stopwatch 对象并开始计时
    public SpeedTimer()
    {
        _stopwatch = new Stopwatch();
        _stopwatch.Start();
    }

    # 记录当前时间并重置计时器
    public void Record(string name)
    {
        # 将当前时间记录到字典中，键为名称，值为计时器的已用时间
        _timeRecordDic[name] = _stopwatch.Elapsed;
        # 重启计时器以进行下一个计时
        _stopwatch.Restart();
    }

    # 打印调试信息
    public void DebugPrint()
    {
        # 初始化一个空的字符串用于存储信息
        var msg = string.Empty;
        # 遍历字典中的每一项
        foreach (var pair in _timeRecordDic)
        {
            # 将每一项的键和值格式化为字符串并添加到 msg 中
            msg += $"{pair.Key}:{pair.Value.TotalMilliseconds}ms,";
        }

        # 如果 msg 不为空，则输出到调试窗口
        if (msg.Length > 0)
        {
            Debug.WriteLine(msg[..^1]);
        }
        # 停止计时器
        _stopwatch.Stop();
    }
}
```