# `.\better-genshin-impact\BetterGenshinImpact\GameTask\AutoSkip\OneKeyExpeditionTask.cs`

```cs
using BetterGenshinImpact.Core.Simulator; // 引入模拟器核心库
using BetterGenshinImpact.GameTask.AutoGeniusInvokation.Exception; // 引入异常处理类
using BetterGenshinImpact.GameTask.AutoSkip.Assets; // 引入自动跳过任务所需资源
using BetterGenshinImpact.GameTask.Common; // 引入通用任务库
using BetterGenshinImpact.View.Drawable; // 引入可绘制视图的相关类
using Microsoft.Extensions.Logging; // 引入日志记录库
using System; // 引入系统基本功能
using static BetterGenshinImpact.GameTask.Common.TaskControl; // 引入任务控制的静态方法
using static Vanara.PInvoke.User32; // 引入 Windows 用户界面的静态方法

namespace BetterGenshinImpact.GameTask.AutoSkip; // 定义命名空间

public class OneKeyExpeditionTask // 定义 OneKeyExpeditionTask 类
{
    public void Run(AutoSkipAssets assets) // 定义 Run 方法，接受 AutoSkipAssets 对象作为参数
    {
        try // 尝试执行以下代码块
        {
            SystemControl.ActivateWindow(); // 激活目标窗口
            // 1.全部领取
            var region = CaptureToRectArea(); // 捕获当前窗口区域
            region.Find(assets.CollectRo, ra => // 查找符合资产 CollectRo 的区域
            {
                ra.Click(); // 点击找到的区域
                Logger.LogInformation("探索派遣：{Text}", "全部领取"); // 记录日志
                Sleep(1100); // 暂停 1.1 秒
                // 2.重新派遣
                NewRetry.Do(() => // 执行重试逻辑
                {
                    Sleep(1); // 暂停 1 毫秒
                    region = CaptureToRectArea(); // 重新捕获当前窗口区域
                    var ra2 = region.Find(assets.ReRo); // 查找符合资产 ReRo 的区域
                    if (ra2.IsEmpty()) // 如果找不到区域
                    {
                        throw new RetryException("未检测到弹出菜单"); // 抛出重试异常
                    }
                    else
                    {
                        ra2.Click(); // 点击找到的区域
                        Logger.LogInformation("探索派遣：{Text}", "再次派遣"); // 记录日志
                    }
                }, TimeSpan.FromSeconds(1), 3); // 设置重试间隔为 1 秒，最大重试次数为 3

                // 3.退出派遣页面 ESC
                Sleep(500); // 暂停 0.5 秒
                Simulation.SendInput.Keyboard.KeyPress(VK.VK_ESCAPE); // 发送 ESC 键输入以退出
                Logger.LogInformation("探索派遣：{Text}", "完成"); // 记录日志
            });
        }
        catch (Exception e) // 捕捉异常
        {
            Logger.LogInformation(e.Message); // 记录异常消息
        }
        finally // 最终执行以下代码块
        {
            VisionContext.Instance().DrawContent.ClearAll(); // 清除绘制内容
        }
    }
}
```