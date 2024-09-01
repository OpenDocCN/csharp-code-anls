# `.\better-genshin-impact\BetterGenshinImpact\Core\Script\Dependence\Genshin.cs`

```cs
# 引入 BetterGenshinImpact.GameTask.AutoTrackPath 命名空间中的类和方法
using BetterGenshinImpact.GameTask.AutoTrackPath;
# 引入 System.Threading.Tasks 命名空间中的类和方法
using System.Threading.Tasks;

# 定义 BetterGenshinImpact.Core.Script.Dependence 命名空间
namespace BetterGenshinImpact.Core.Script.Dependence;

# 定义 Genshin 类
public class Genshin
{
    # 定义一个异步方法 Tp，接收两个 double 类型的参数 x 和 y
    public async Task Tp(double x, double y)
    {
        # 创建 TpTask 对象并传入 CancellationContext.Instance.Cts
        # 调用 Tp 方法，并传入 x 和 y
        await new TpTask(CancellationContext.Instance.Cts).Tp(x, y);
    }

    # 定义一个异步方法 Tp，接收两个 string 类型的参数 x 和 y
    public async Task Tp(string x, string y)
    {
        # 尝试将字符串 x 转换为 double 类型，并存储在 dx 变量中
        double.TryParse(x, out var dx);
        # 尝试将字符串 y 转换为 double 类型，并存储在 dy 变量中
        double.TryParse(y, out var dy);
        # 调用另一个 Tp 方法，传入转换后的 dx 和 dy
        await Tp(dx, dy);
    }
}
```