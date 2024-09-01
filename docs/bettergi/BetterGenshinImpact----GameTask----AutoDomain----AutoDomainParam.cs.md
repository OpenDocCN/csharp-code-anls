# `.\better-genshin-impact\BetterGenshinImpact\GameTask\AutoDomain\AutoDomainParam.cs`

```cs
# 引入 BetterGenshinImpact.GameTask.Model 命名空间中的类
using BetterGenshinImpact.GameTask.Model;
# 引入 System.Threading 命名空间中的类
using System.Threading;

# 定义 BetterGenshinImpact.GameTask.AutoDomain 命名空间
namespace BetterGenshinImpact.GameTask.AutoDomain;

# 定义 AutoDomainParam 类，继承自 BaseTaskParam
public class AutoDomainParam : BaseTaskParam
{
    # 定义一个公共的整型属性 DomainRoundNum
    public int DomainRoundNum { get; set; }

    # 定义一个公共的字符串属性 CombatStrategyPath
    public string CombatStrategyPath { get; set; }

    # 构造函数，接收 CancellationTokenSource 对象、整型参数 domainRoundNum 和字符串参数 path
    public AutoDomainParam(CancellationTokenSource cts, int domainRoundNum, string path) : base(cts)
    {
        # 初始化 DomainRoundNum 属性
        DomainRoundNum = domainRoundNum;
        # 如果 domainRoundNum 为 0，则将 DomainRoundNum 设置为 9999
        if (domainRoundNum == 0)
        {
            DomainRoundNum = 9999;
        }
        # 初始化 CombatStrategyPath 属性
        CombatStrategyPath = path;
    }
}
```