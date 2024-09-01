# `.\better-genshin-impact\BetterGenshinImpact\Core\Monitor\MouseKeyMonitor.cs`

```cs
# 引用 BetterGenshinImpact.Core.Recorder 命名空间中的类
﻿using BetterGenshinImpact.Core.Recorder;
# 引用 BetterGenshinImpact.Core.Simulator 命名空间中的类
using BetterGenshinImpact.Core.Simulator;
# 引用 BetterGenshinImpact.GameTask 命名空间中的类
using BetterGenshinImpact.GameTask;
# 引用 BetterGenshinImpact.Model 命名空间中的类
using BetterGenshinImpact.Model;
# 引用 Gma.System.MouseKeyHook 命名空间中的类
using Gma.System.MouseKeyHook;
# 引用 System 命名空间中的类
using System;
# 引用 System.Diagnostics 命名空间中的类
using System.Diagnostics;
# 引用 System.Windows.Forms 命名空间中的类
using System.Windows.Forms;
# 引用 Vanara.PInvoke 命名空间中的类
using Vanara.PInvoke;
# 引用 Timer 类并指定其命名空间
using Timer = System.Timers.Timer;

# 定义一个命名空间 BetterGenshinImpact.Core.Monitor
namespace BetterGenshinImpact.Core.Monitor;

# 定义一个 MouseKeyMonitor 类
public class MouseKeyMonitor
{
    # 定义一个定时器，用于长按 F 键时变成 F 连发
    private readonly Timer _fTimer = new();

    # 定义一个定时器，用于长按空格键时变成空格键连发
    private readonly Timer _spaceTimer = new();

    # 记录第一次按下 F 键的时间
    private DateTime _firstFKeyDownTime = DateTime.MaxValue;

    # 记录第一次按下空格键的时间，DateTime.MaxValue 表示没有按下
    private DateTime _firstSpaceKeyDownTime = DateTime.MaxValue;

    # 定义一个全局的键盘鼠标事件对象
    private IKeyboardMouseEvents? _globalHook;
    # 定义一个窗口句柄
    private nint _hWnd;

    # 订阅系统级的鼠标键盘事件
    public void Subscribe(nint gameHandle)
    {
        # 设置窗口句柄
        _hWnd = gameHandle;
        # 创建全局事件钩子
        _globalHook = Hook.GlobalEvents();

        # 绑定全局钩子的键盘按下事件处理函数
        _globalHook.KeyDown += GlobalHookKeyDown;
        # 绑定全局钩子的键盘松开事件处理函数
        _globalHook.KeyUp += GlobalHookKeyUp;
        # 绑定全局钩子的鼠标按下事件处理函数
        _globalHook.MouseDownExt += GlobalHookMouseDownExt;
        # 绑定全局钩子的鼠标松开事件处理函数
        _globalHook.MouseUpExt += GlobalHookMouseUpExt;
        # 绑定全局钩子的鼠标移动事件处理函数
        _globalHook.MouseMoveExt += GlobalHookMouseMoveExt;
        # 注释掉全局钩子的键盘按键事件处理函数
        //_globalHook.KeyPress += GlobalHookKeyPress;

        # 初始化第一次按下空格键的时间
        _firstSpaceKeyDownTime = DateTime.MaxValue;
        # 从配置中获取空格键连发的时间间隔
        var si = TaskContext.Instance().Config.MacroConfig.SpaceFireInterval;
        # 设置空格键定时器的时间间隔
        _spaceTimer.Interval = si;
        # 绑定定时器的 Elapsed 事件处理函数
        _spaceTimer.Elapsed += (sender, args) => { Simulation.PostMessage(_hWnd).KeyPress(User32.VK.VK_SPACE); };

        # 从配置中获取 F 键连发的时间间隔
        var fi = TaskContext.Instance().Config.MacroConfig.FFireInterval;
        # 设置 F 键定时器的时间间隔
        _fTimer.Interval = fi;
        # 绑定定时器的 Elapsed 事件处理函数
        _fTimer.Elapsed += (sender, args) => { Simulation.PostMessage(_hWnd).KeyPress(User32.VK.VK_F); };
    }

    # 定义键盘按下事件的处理函数
    private void GlobalHookKeyDown(object? sender, KeyEventArgs e)
    {
        // 调用全局钩子记录实例处理按键按下事件
        GlobalKeyMouseRecord.Instance.GlobalHookKeyDown(e);

        // 触发热键按下事件处理方法
        HotKeyDown(sender, e);

        // 如果按下的是空格键
        if (e.KeyCode == Keys.Space)
        {
            // 如果第一次按下时间未被设置
            if (_firstSpaceKeyDownTime == DateTime.MaxValue)
            {
                // 设置第一次按下时间为当前时间
                _firstSpaceKeyDownTime = DateTime.Now;
            }
            else
            {
                // 计算空格键被按下的时间间隔
                var timeSpan = DateTime.Now - _firstSpaceKeyDownTime;
                // 如果时间间隔大于300毫秒，并且配置启用了“空格按住以继续”
                if (timeSpan.TotalMilliseconds > 300 && TaskContext.Instance().Config.MacroConfig.SpacePressHoldToContinuationEnabled)
                    // 如果计时器未启动
                    if (!_spaceTimer.Enabled)
                        // 启动计时器
                        _spaceTimer.Start();
            }
        }
        // 如果按下的是F键
        else if (e.KeyCode == Keys.F)
        {
            // 如果第一次按下时间未被设置
            if (_firstFKeyDownTime == DateTime.MaxValue)
            {
                // 设置第一次按下时间为当前时间
                _firstFKeyDownTime = DateTime.Now;
            }
            else
            {
                // 计算F键被按下的时间间隔
                var timeSpan = DateTime.Now - _firstFKeyDownTime;
                // 如果时间间隔大于200毫秒，并且配置启用了“F按住以继续”
                if (timeSpan.TotalMilliseconds > 200 && TaskContext.Instance().Config.MacroConfig.FPressHoldToContinuationEnabled)
                    // 如果计时器未启动
                    if (!_fTimer.Enabled)
                        // 启动计时器
                        _fTimer.Start();
            }
        }
    }

    private void GlobalHookKeyUp(object? sender, KeyEventArgs e)
    {
        // 调用全局钩子记录实例处理按键松开事件
        GlobalKeyMouseRecord.Instance.GlobalHookKeyUp(e);

        // 触发热键松开事件处理方法
        HotKeyUp(sender, e);

        // 如果松开的是空格键
        if (e.KeyCode == Keys.Space)
        {
            // 如果第一次按下时间已经被设置
            if (_firstSpaceKeyDownTime != DateTime.MaxValue)
            {
                // 计算空格键被按下的时间间隔并打印
                var timeSpan = DateTime.Now - _firstSpaceKeyDownTime;
                Debug.WriteLine($"Space按下时间：{timeSpan.TotalMilliseconds}ms");
                // 重置第一次按下时间
                _firstSpaceKeyDownTime = DateTime.MaxValue;
                // 停止空格计时器
                _spaceTimer.Stop();
            }
        }
        // 如果松开的是F键
        else if (e.KeyCode == Keys.F)
        {
            // 如果第一次按下时间已经被设置
            if (_firstFKeyDownTime != DateTime.MaxValue)
            {
                // 计算F键被按下的时间间隔并打印
                var timeSpan = DateTime.Now - _firstFKeyDownTime;
                Debug.WriteLine($"F按下时间：{timeSpan.TotalMilliseconds}ms");
                // 重置第一次按下时间
                _firstFKeyDownTime = DateTime.MaxValue;
                // 停止F计时器
                _fTimer.Stop();
            }
        }
    }

    private void HotKeyDown(object? sender, KeyEventArgs e)
    {
        // 如果键码存在于所有键盘钩子中，调用对应的KeyDown处理方法
        if (KeyboardHook.AllKeyboardHooks.TryGetValue(e.KeyCode, out var hook)) hook.KeyDown(sender, e);
    }

    private void HotKeyUp(object? sender, KeyEventArgs e)
    {
        // 如果键码存在于所有键盘钩子中，调用对应的KeyUp处理方法
        if (KeyboardHook.AllKeyboardHooks.TryGetValue(e.KeyCode, out var hook)) hook.KeyUp(sender, e);
    }

    //private void GlobalHookKeyPress(object? sender, KeyPressEventArgs e)
    //{
    //    // 调试输出按键字符
    //    Debug.WriteLine("KeyPress: \t{0}", e.KeyChar);
    //}

    private void GlobalHookMouseDownExt(object? sender, MouseEventExtArgs e)
    {
        // Debug.WriteLine("MouseDown: {0}; \t Location: {1};\t System Timestamp: {2}", e.Button, e.Location, e.Timestamp);
        // 调用全局键鼠记录实例的 MouseDown 方法处理鼠标按下事件
        GlobalKeyMouseRecord.Instance.GlobalHookMouseDown(e);

        // 检查鼠标按钮是否不是左键
        if (e.Button != MouseButtons.Left)
            // 如果 AllMouseHooks 中包含该按钮的钩子
            if (MouseHook.AllMouseHooks.TryGetValue(e.Button, out var hook))
                // 调用相应的钩子的 MouseDown 方法
                hook.MouseDown(sender, e);
    }

    private void GlobalHookMouseUpExt(object? sender, MouseEventExtArgs e)
    {
        // Debug.WriteLine("MouseUp: {0}; \t Location: {1};\t System Timestamp: {2}", e.Button, e.Location, e.Timestamp);
        // 调用全局键鼠记录实例的 MouseUp 方法处理鼠标抬起事件
        GlobalKeyMouseRecord.Instance.GlobalHookMouseUp(e);

        // 检查鼠标按钮是否不是左键
        if (e.Button != MouseButtons.Left)
            // 如果 AllMouseHooks 中包含该按钮的钩子
            if (MouseHook.AllMouseHooks.TryGetValue(e.Button, out var hook))
                // 调用相应的钩子的 MouseUp 方法
                hook.MouseUp(sender, e);
    }

    private void GlobalHookMouseMoveExt(object? sender, MouseEventExtArgs e)
    {
        // Debug.WriteLine("MouseMove: {0}; \t Location: {1};\t System Timestamp: {2}", e.Button, e.Location, e.Timestamp);
        // 调用全局键鼠记录实例的 MouseMoveTo 方法处理鼠标移动事件
        GlobalKeyMouseRecord.Instance.GlobalHookMouseMoveTo(e);
    }

    public void Unsubscribe()
    {
        // 如果 _globalHook 不为空
        if (_globalHook != null)
        {
            // 从 _globalHook 中移除 KeyDown 事件的订阅
            _globalHook.KeyDown -= GlobalHookKeyDown;
            // 从 _globalHook 中移除 KeyUp 事件的订阅
            _globalHook.KeyUp -= GlobalHookKeyUp;
            // 从 _globalHook 中移除 MouseDownExt 事件的订阅
            _globalHook.MouseDownExt -= GlobalHookMouseDownExt;
            // 从 _globalHook 中移除 MouseUpExt 事件的订阅
            _globalHook.MouseUpExt -= GlobalHookMouseUpExt;
            // 从 _globalHook 中移除 MouseMoveExt 事件的订阅
            _globalHook.MouseMoveExt -= GlobalHookMouseMoveExt;
            //_globalHook.KeyPress -= GlobalHookKeyPress; // 注释掉的代码，用于移除 KeyPress 事件的订阅
            // 释放 _globalHook 资源
            _globalHook.Dispose();
        }
    }
这个代码片段似乎是不完整的，可能是一些错误或遗漏。如果您能提供完整的代码或上下文，我可以更好地为您添加注释。
```