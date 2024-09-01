# `.\better-genshin-impact\BetterGenshinImpact\Core\Recorder\GlobalKeyMouseRecord.cs`

```cs
﻿using BetterGenshinImpact.Core.Monitor; // 引入监视器相关的命名空间
using BetterGenshinImpact.GameTask; // 引入游戏任务相关的命名空间
using BetterGenshinImpact.GameTask.Common; // 引入通用游戏任务相关的命名空间
using BetterGenshinImpact.GameTask.Common.Element.Assets; // 引入游戏任务中的资产相关命名空间
using BetterGenshinImpact.Model; // 引入模型相关的命名空间
using Gma.System.MouseKeyHook; // 引入鼠标键盘钩子库的命名空间
using Microsoft.Extensions.Logging; // 引入日志记录相关的命名空间
using SharpDX.DirectInput; // 引入 DirectInput 相关的命名空间
using System; // 引入系统基本功能相关的命名空间
using System.Collections.Generic; // 引入集合类的命名空间
using System.Threading.Tasks; // 引入任务并行处理相关的命名空间
using System.Windows.Forms; // 引入 Windows 窗体相关的命名空间
using Wpf.Ui.Violeta.Controls; // 引入 WPF UI 控件的命名空间

namespace BetterGenshinImpact.Core.Recorder; // 定义命名空间

public class GlobalKeyMouseRecord : Singleton<GlobalKeyMouseRecord> // 定义全局键鼠记录类，继承 Singleton
{
    private readonly ILogger<GlobalKeyMouseRecord> _logger = App.GetLogger<GlobalKeyMouseRecord>(); // 创建日志记录器实例

    private KeyMouseRecorder? _recorder; // 声明键鼠记录器变量

    private readonly Dictionary<Keys, bool> _keyDownState = []; // 声明键按下状态的字典

    private DirectInputMonitor? _directInputMonitor; // 声明 DirectInput 监视器变量

    private readonly System.Timers.Timer _timer = new(); // 创建计时器实例

    private bool _isInMainUi = false; // 是否在主界面的标志

    public KeyMouseRecorderStatus Status { get; set; } = KeyMouseRecorderStatus.Stop; // 键鼠记录器状态属性，初始为停止状态

    public GlobalKeyMouseRecord() // 构造函数
    {
        _timer.Elapsed += Tick; // 订阅计时器的时间到期事件
        _timer.Interval = 50; // 设置计时器的间隔时间为 50 毫秒
    }

    public async Task StartRecord() // 异步开始记录的方法
    {
        if (!TaskContext.Instance().IsInitialized) // 检查任务上下文是否初始化
        {
            Toast.Warning("请先在启动页，启动截图器再使用本功能"); // 弹出警告提示
            return; // 退出方法
        }

        if (Status != KeyMouseRecorderStatus.Stop) // 检查当前状态是否为停止
        {
            Toast.Warning("已经在录制状态，请不要重复启动录制功能"); // 弹出警告提示
            return; // 退出方法
        }

        Status = KeyMouseRecorderStatus.Start; // 更新状态为启动

        SystemControl.ActivateWindow(); // 激活系统窗口

        _logger.LogInformation("录制：{Text}", "实时任务已暂停"); // 记录日志信息
        _logger.LogInformation("注意：录制时遇到主界面（鼠标永远在界面中心）和其他界面（鼠标可自由移动，比如地图等）的切换，请把手离开鼠标等待录制模式切换日志"); // 记录日志信息

        for (var i = 3; i >= 1; i--) // 倒计时循环
        {
            _logger.LogInformation("{Sec}秒后启动录制...", i); // 记录倒计时信息
            await Task.Delay(1000); // 异步等待 1 秒
        }

        TaskTriggerDispatcher.Instance().StopTimer(); // 停止任务触发调度器的计时器

        _timer.Start(); // 启动计时器

        _recorder = new KeyMouseRecorder(); // 初始化键鼠记录器
        _directInputMonitor = new DirectInputMonitor(); // 初始化 DirectInput 监视器
        _directInputMonitor.Start(); // 启动 DirectInput 监视器

        Status = KeyMouseRecorderStatus.Recording; // 更新状态为录制中

        _logger.LogInformation("录制：{Text}", "已启动"); // 记录日志信息
    }

    public string StopRecord() // 停止记录的方法
    {
        if (Status != KeyMouseRecorderStatus.Recording) // 检查当前状态是否为录制中
        {
            throw new InvalidOperationException("未处于录制中状态，无法停止"); // 抛出异常
        }

        var macro = _recorder?.ToJsonMacro() ?? string.Empty; // 获取记录的宏数据并转为 JSON 格式，若为空则返回空字符串
        _recorder = null; // 清空记录器实例
        _directInputMonitor?.Stop(); // 停止 DirectInput 监视器
        _directInputMonitor?.Dispose(); // 释放 DirectInput 监视器资源
        _directInputMonitor = null; // 清空 DirectInput 监视器实例

        _timer.Stop(); // 停止计时器

        _logger.LogInformation("录制：{Text}", "结束录制"); // 记录日志信息

        TaskTriggerDispatcher.Instance().StartTimer(); // 启动任务触发调度器的计时器

        Status = KeyMouseRecorderStatus.Stop; // 更新状态为停止

        return macro; // 返回宏数据
    }

    public void Tick(object? sender, EventArgs e) // 计时器触发的事件处理方法
    # 捕获当前屏幕区域的矩形
    var ra = TaskControl.CaptureToRectArea();
    # 查找在友聊元素中的矩形区域
    var iconRa = ra.Find(ElementAssets.Instance.FriendChat);
    # 检查找到的区域是否存在
    var exist = iconRa.IsExist();
    # 如果找到的存在状态与当前 UI 状态不同，则记录信息
    if (exist != _isInMainUi)
    {
        _logger.LogInformation("录制：{Text}", exist ? "进入主界面，捕获鼠标相对移动" : "离开主界面，捕获鼠标绝对移动");
    }
    # 更新当前 UI 状态
    _isInMainUi = exist;
    # 释放图标区域资源
    iconRa.Dispose();
    # 释放屏幕区域资源
    ra.Dispose();
    # 按下键事件处理
    public void GlobalHookKeyDown(KeyEventArgs e)
    {
        # 如果是热键，则直接返回
        if (e.KeyCode.ToString() == TaskContext.Instance().Config.HotKeyConfig.KeyMouseMacroRecordHotkey)
        {
            return;
        }

        # 检查键是否已经按下
        if (_keyDownState.TryGetValue(e.KeyCode, out var v))
        {
            if (v)
            {
                return; # 已经按下状态，不再记录
            }
            else
            {
                # 更新键的状态为按下
                _keyDownState[e.KeyCode] = true;
            }
        }
        else
        {
            # 添加新的按下键状态
            _keyDownState.Add(e.KeyCode, true);
        }
        # 调用记录器的 KeyDown 方法
        _recorder?.KeyDown(e);
    }

    # 松开键事件处理
    public void GlobalHookKeyUp(KeyEventArgs e)
    {
        # 如果是特定热键，则直接返回
        if (e.KeyCode.ToString() == TaskContext.Instance().Config.HotKeyConfig.Test1Hotkey)
        {
            return;
        }

        # 检查键是否处于按下状态并更新为松开状态
        if (_keyDownState.TryGetValue(e.KeyCode, out bool state) && state)
        {
            # 调用记录器的 KeyUp 方法
            _keyDownState[e.KeyCode] = false;
            _recorder?.KeyUp(e);
        }
    }

    # 鼠标按下事件处理
    public void GlobalHookMouseDown(MouseEventExtArgs e)
    {
        # 调用记录器的 MouseDown 方法
        _recorder?.MouseDown(e);
    }

    # 鼠标松开事件处理
    public void GlobalHookMouseUp(MouseEventExtArgs e)
    {
        # 调用记录器的 MouseUp 方法
        _recorder?.MouseUp(e);
    }

    # 鼠标移动到指定位置事件处理
    public void GlobalHookMouseMoveTo(MouseEventExtArgs e)
    {
        # 如果不在主 UI 界面，则直接返回
        if (_isInMainUi)
        {
            return;
        }
        # 调用记录器的 MouseMoveTo 方法
        _recorder?.MouseMoveTo(e);
    }

    # 鼠标相对移动事件处理
    public void GlobalHookMouseMoveBy(MouseState state)
    {
        # 如果移动量为零或不在主 UI 界面，则直接返回
        if (state is { X: 0, Y: 0 } || !_isInMainUi)
        {
            return;
        }
        # 调用记录器的 MouseMoveBy 方法
        _recorder?.MouseMoveBy(state);
    }
}
# 结束前一个代码块的闭合标记（例如一个类或方法的结束）

public enum KeyMouseRecorderStatus
{
    # 定义一个名为 KeyMouseRecorderStatus 的枚举类型
    Start,        # 枚举值，表示录制开始状态
    Recording,    # 枚举值，表示录制进行中状态
    Stop          # 枚举值，表示录制停止状态
}
```