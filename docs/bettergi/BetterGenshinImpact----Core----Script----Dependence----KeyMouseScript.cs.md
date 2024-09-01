# `.\better-genshin-impact\BetterGenshinImpact\Core\Script\Dependence\KeyMouseScript.cs`

```cs
# 引入 BetterGenshinImpact.Core.Recorder 命名空间中的内容
﻿using BetterGenshinImpact.Core.Recorder;
# 引入 System.Threading.Tasks 命名空间中的内容
using System.Threading.Tasks;

# 定义 BetterGenshinImpact.Core.Script.Dependence 命名空间
namespace BetterGenshinImpact.Core.Script.Dependence;

# 定义 KeyMouseScript 类，并带有一个构造函数参数 rootPath
public class KeyMouseScript(string rootPath)
{
    # 定义异步方法 Run，接受一个 JSON 字符串作为参数
    public async Task Run(string json)
    {
        # 调用 KeyMouseMacroPlayer 的 PlayMacro 方法，传入 JSON 字符串、取消令牌和布尔值 false
        await KeyMouseMacroPlayer.PlayMacro(json, CancellationContext.Instance.Cts.Token, false);
    }

    # 定义异步方法 RunFile，接受一个路径作为参数
    public async Task RunFile(string path)
    {
        # 创建 LimitedFile 对象，并读取指定路径的文本内容
        var json = await new LimitedFile(rootPath).ReadText(path);
        # 调用 Run 方法，传入读取到的 JSON 字符串
        await Run(json);
    }
}
```