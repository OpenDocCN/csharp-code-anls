# `.\better-genshin-impact\BetterGenshinImpact\GameTask\Macro\TurnAroundMacro.cs`

```cs
# 引入 BetterGenshinImpact.Core.Simulator 命名空间
using BetterGenshinImpact.Core.Simulator;
# 引入 System.Threading 命名空间
using System.Threading;

# 定义 BetterGenshinImpact.GameTask.Macro 命名空间
namespace BetterGenshinImpact.GameTask.Macro
{
    # 定义 TurnAroundMacro 类
    public class TurnAroundMacro
    {
        # 定义静态方法 Done
        public static void Done()
        {
            # 如果 RunaroundMouseXInterval 为 0，则将其设置为 1
            if (TaskContext.Instance().Config.MacroConfig.RunaroundMouseXInterval == 0)
            {
                TaskContext.Instance().Config.MacroConfig.RunaroundMouseXInterval = 1;
            }

            # 移动鼠标，水平移动距离由 RunaroundMouseXInterval 决定
            Simulation.SendInput.Mouse.MoveMouseBy(TaskContext.Instance().Config.MacroConfig.RunaroundMouseXInterval, 0);
            # 线程休眠，时间由 RunaroundInterval 决定
            Thread.Sleep(TaskContext.Instance().Config.MacroConfig.RunaroundInterval);
        }
    }
}
```