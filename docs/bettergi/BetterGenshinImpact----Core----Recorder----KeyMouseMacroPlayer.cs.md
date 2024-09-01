# `.\better-genshin-impact\BetterGenshinImpact\Core\Recorder\KeyMouseMacroPlayer.cs`

```cs
﻿using BetterGenshinImpact.Core.Recorder.Model; // 引用 BetterGenshinImpact.Core.Recorder.Model 命名空间中的模型
using BetterGenshinImpact.Core.Simulator; // 引用 BetterGenshinImpact.Core.Simulator 命名空间中的模拟器
using BetterGenshinImpact.GameTask; // 引用 BetterGenshinImpact.GameTask 命名空间中的游戏任务功能
using BetterGenshinImpact.GameTask.Common; // 引用 BetterGenshinImpact.GameTask.Common 命名空间中的通用游戏任务功能
using BetterGenshinImpact.GameTask.Common.Map; // 引用 BetterGenshinImpact.GameTask.Common.Map 命名空间中的地图功能
using BetterGenshinImpact.Helpers; // 引用 BetterGenshinImpact.Helpers 命名空间中的辅助功能
using Microsoft.Extensions.Logging; // 引用 Microsoft.Extensions.Logging 命名空间中的日志功能
using System; // 引用 System 命名空间中的基本类型和功能
using System.Collections.Generic; // 引用 System.Collections.Generic 命名空间中的泛型集合功能
using System.Drawing; // 引用 System.Drawing 命名空间中的绘图功能
using System.Text.Json; // 引用 System.Text.Json 命名空间中的 JSON 序列化和反序列化功能
using System.Threading; // 引用 System.Threading 命名空间中的线程功能
using System.Threading.Tasks; // 引用 System.Threading.Tasks 命名空间中的异步任务功能
using System.Windows.Forms; // 引用 System.Windows.Forms 命名空间中的 Windows 窗体功能
using Vanara.PInvoke; // 引用 Vanara.PInvoke 命名空间中的 P/Invoke 功能
using Wpf.Ui.Violeta.Controls; // 引用 Wpf.Ui.Violeta.Controls 命名空间中的 WPF 控件

namespace BetterGenshinImpact.Core.Recorder; // 定义 BetterGenshinImpact.Core.Recorder 命名空间

public class KeyMouseMacroPlayer // 定义 KeyMouseMacroPlayer 类
{
    public static async Task PlayMacro(string macro, CancellationToken ct, bool withDelay = true) // 定义异步方法 PlayMacro，用于播放宏
    {
        if (!TaskContext.Instance().IsInitialized) // 检查任务上下文是否已初始化
        {
            Toast.Warning("请先在启动页，启动截图器再使用本功能"); // 提示用户先启动截图器
            return; // 退出方法
        }

        var script = JsonSerializer.Deserialize<KeyMouseScript>(macro, KeyMouseRecorder.JsonOptions) ?? throw new Exception("Failed to deserialize macro"); // 反序列化宏字符串为 KeyMouseScript 对象，若失败则抛出异常
        script.Adapt(TaskContext.Instance().SystemInfo.CaptureAreaRect, TaskContext.Instance().DpiScale); // 调整脚本适应当前系统信息
        SystemControl.ActivateWindow(); // 激活当前窗口

        if (withDelay) // 如果需要延迟
        {
            for (var i = 3; i >= 1; i--) // 从 3 秒倒计时
            {
                TaskControl.Logger.LogInformation("{Sec}秒后进行重放...", i); // 记录倒计时信息
                await Task.Delay(1000, ct); // 等待 1 秒
            }

            TaskControl.Logger.LogInformation("开始重放"); // 记录重放开始的信息
        }

        await PlayMacro(script.MacroEvents, ct); // 异步播放宏事件
    }

    public static async Task PlayMacro(List<MacroEvent> macroEvents, CancellationToken ct) // 定义异步方法 PlayMacro，用于播放宏事件列表
    {
        // 方法内容未提供
    }

    public static Size WorkingArea; // 定义静态字段 WorkingArea，表示工作区域大小

    public static double ToVirtualDesktopX(int x) // 定义方法，将屏幕坐标 x 转换为虚拟桌面坐标 x
    {
        return x * 65535 * 1d / WorkingArea.Width; // 计算虚拟桌面坐标
    }

    public static double ToVirtualDesktopY(int y) // 定义方法，将屏幕坐标 y 转换为虚拟桌面坐标 y
    {
        return y * 65535 * 1d / WorkingArea.Height; // 计算虚拟桌面坐标
    }
}
```