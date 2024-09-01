# `.\better-genshin-impact\BetterGenshinImpact\Core\Simulator\Simulation.cs`

```cs
# 导入 Fischless.WindowsInput 和 System 命名空间
﻿using Fischless.WindowsInput;
using System;

# 定义命名空间 BetterGenshinImpact.Core.Simulator
namespace BetterGenshinImpact.Core.Simulator;

# 定义 Simulation 类
public class Simulation
{
    # 静态属性 SendInput，初始化为 InputSimulator 的新实例
    public static InputSimulator SendInput { get; } = new();

    # 静态属性 MouseEvent，初始化为 MouseEventSimulator 的新实例
    public static MouseEventSimulator MouseEvent { get; } = new();

    # 静态方法 PostMessage，接受窗口句柄并返回 PostMessageSimulator 实例
    public static PostMessageSimulator PostMessage(IntPtr hWnd)
    {
        # 创建并返回 PostMessageSimulator 的新实例，传入窗口句柄
        return new PostMessageSimulator(hWnd);
    }
}
```