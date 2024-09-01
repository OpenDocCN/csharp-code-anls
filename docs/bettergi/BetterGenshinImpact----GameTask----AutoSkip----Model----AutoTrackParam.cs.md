# `.\better-genshin-impact\BetterGenshinImpact\GameTask\AutoSkip\Model\AutoTrackParam.cs`

```cs
# 引入 BetterGenshinImpact.GameTask.Model 命名空间
using BetterGenshinImpact.GameTask.Model;
# 引入 System.Threading 命名空间
using System.Threading;

# 定义 BetterGenshinImpact.GameTask.AutoSkip.Model 命名空间
namespace BetterGenshinImpact.GameTask.AutoSkip.Model
{
    # 定义 AutoTrackParam 类，继承自 BaseTaskParam
    public class AutoTrackParam : BaseTaskParam
    {
        # 构造函数，接受一个 CancellationTokenSource 对象作为参数
        public AutoTrackParam(CancellationTokenSource cts) : base(cts)
        {
        }
    }
}
```