# `.\better-genshin-impact\BetterGenshinImpact\Core\Monitor\DirectInputMonitor.cs`

```cs
﻿using BetterGenshinImpact.Core.Recorder; // 导入 BetterGenshinImpact.Core.Recorder 命名空间
using SharpDX.DirectInput; // 导入 SharpDX.DirectInput 命名空间
using System; // 导入 System 命名空间
using System.Threading; // 导入 System.Threading 命名空间
using System.Threading.Tasks; // 导入 System.Threading.Tasks 命名空间

namespace BetterGenshinImpact.Core.Monitor; // 定义 BetterGenshinImpact.Core.Monitor 命名空间

public class DirectInputMonitor : IDisposable // 定义 DirectInputMonitor 类，并实现 IDisposable 接口
{
    private bool _isRunning = true; // 定义一个布尔字段，指示监控是否运行

    private readonly Mouse _mouse; // 定义一个只读字段，存储鼠标对象

    public DirectInputMonitor() // 构造函数
    {
        var directInput = new DirectInput(); // 创建 DirectInput 实例
        _mouse = new Mouse(directInput); // 使用 DirectInput 实例创建 Mouse 对象
        _mouse.SetCooperativeLevel(IntPtr.Zero, CooperativeLevel.Background | CooperativeLevel.NonExclusive); // 设置鼠标的合作级别为后台和非独占
    }

    public MouseState GetMouseState() // 获取鼠标状态的方法
    {
        _mouse.Acquire(); // 获取鼠标的控制权
        return _mouse.GetCurrentState(); // 返回当前鼠标状态
    }

    public void Start() // 启动监控的方法
    {
        Task.Run(() => // 启动一个新任务
        {
            while (_isRunning) // 当监控正在运行时
            {
                _mouse.Acquire(); // 获取鼠标的控制权
                MouseState state = _mouse.GetCurrentState(); // 获取当前鼠标状态
                // Debug.WriteLine($"{state.X} {state.Y} {state.Buttons[0]} {state.Buttons[1]}"); // （调试用）输出鼠标状态
                GlobalKeyMouseRecord.Instance.GlobalHookMouseMoveBy(state); // 记录鼠标移动
                Thread.Sleep(5); // 暂停5毫秒
            }
        });
    }

    public void Stop() // 停止监控的方法
    {
        _isRunning = false; // 设置监控状态为停止
    }

    public void Dispose() // 释放资源的方法
    {
        _mouse.Dispose(); // 释放鼠标对象
    }
}
```