# `.\better-genshin-impact\BetterGenshinImpact\Core\Script\Dependence\AutoPathing.cs`

```cs
# 引入系统线程任务相关命名空间
﻿using System.Threading.Tasks;
# 引入系统 JSON 序列化相关命名空间
using System.Text.Json;
# 引入 BetterGenshinImpact 的自动路径相关功能
using BetterGenshinImpact.GameTask.AutoPathing;
using BetterGenshinImpact.GameTask.AutoPathing.Model;

# 定义 BetterGenshinImpact.Core.Script.Dependence 命名空间
namespace BetterGenshinImpact.Core.Script.Dependence
{
    # 定义 AutoPathing 类，并且其构造函数接受一个根路径参数
    internal class AutoPathing(string rootPath)
    {
        # 异步方法 Run，接受一个 JSON 字符串
        public async Task Run(string json)
        {
            # 将 JSON 字符串反序列化为 PathingTask 对象
            var task = JsonSerializer.Deserialize<PathingTask>(json);
            # 如果反序列化结果为空，则直接返回
            if (task == null)
            {
                return;
            }
            # 使用 PathExecutor 执行路径规划任务，传入取消令牌
            await PathExecutor.Pathing(task, CancellationContext.Instance.Cts);
        }

        # 异步方法 RunFile，接受一个文件路径
        public async Task RunFile(string path)
        {
            # 读取指定路径的文件内容，并返回其文本
            var json = await new LimitedFile(rootPath).ReadText(path);
            # 调用 Run 方法处理读取到的 JSON 数据
            await Run(json);
        }
    }
}
```