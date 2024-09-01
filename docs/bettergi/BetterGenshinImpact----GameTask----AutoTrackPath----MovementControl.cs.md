# `.\better-genshin-impact\BetterGenshinImpact\GameTask\AutoTrackPath\MovementControl.cs`

```cs
# 引入 BetterGenshinImpact 核心模拟器库
﻿using BetterGenshinImpact.Core.Simulator;
# 引入 BetterGenshinImpact 模型库
using BetterGenshinImpact.Model;
# 引入 Vanara.PInvoke 库
using Vanara.PInvoke;

# 定义自动跟踪路径的命名空间
namespace BetterGenshinImpact.GameTask.AutoTrackPath;

# 定义 MovementControl 类，继承自 Singleton<MovementControl>
public class MovementControl : Singleton<MovementControl>
{
    # 定义私有布尔变量 _wDown，初始值为 false
    private bool _wDown = false;

    # 定义 WDown 方法
    public void WDown()
    {
        # 如果 _wDown 为 false，则设置为 true，并模拟键盘 W 键按下
        if (!_wDown)
        {
            _wDown = true;
            Simulation.SendInput.Keyboard.KeyDown(User32.VK.VK_W);
        }
    }

    # 定义 WUp 方法
    public void WUp()
    {
        # 如果 _wDown 为 true，则设置为 false，并模拟键盘 W 键松开
        if (_wDown)
        {
            _wDown = false;
            Simulation.SendInput.Keyboard.KeyUp(User32.VK.VK_W);
        }
    }

    # 定义 SpacePress 方法
    public void SpacePress()
    {
        # 模拟键盘空格键按下和释放
        Simulation.SendInput.Keyboard.KeyPress(User32.VK.VK_SPACE);
    }
}
```