# `.\better-genshin-impact\BetterGenshinImpact\GameTask\AutoFight\OneKeyFightTask.cs`

```cs
﻿using BetterGenshinImpact.Core.Config; // 引入 BetterGenshinImpact.Core.Config 命名空间
using BetterGenshinImpact.GameTask.AutoFight.Model; // 引入 BetterGenshinImpact.GameTask.AutoFight.Model 命名空间
using BetterGenshinImpact.GameTask.AutoFight.Script; // 引入 BetterGenshinImpact.GameTask.AutoFight.Script 命名空间
using BetterGenshinImpact.GameTask.Model.Enum; // 引入 BetterGenshinImpact.GameTask.Model.Enum 命名空间
using BetterGenshinImpact.Model; // 引入 BetterGenshinImpact.Model 命名空间
using BetterGenshinImpact.Service; // 引入 BetterGenshinImpact.Service 命名空间
using Microsoft.Extensions.Logging; // 引入 Microsoft.Extensions.Logging 命名空间
using System; // 引入 System 命名空间
using System.Collections.Generic; // 引入 System.Collections.Generic 命名空间
using System.IO; // 引入 System.IO 命名空间
using System.Linq; // 引入 System.Linq 命名空间
using System.Text.Json; // 引入 System.Text.Json 命名空间
using System.Threading; // 引入 System.Threading 命名空间
using System.Threading.Tasks; // 引入 System.Threading.Tasks 命名空间
using static BetterGenshinImpact.GameTask.Common.TaskControl; // 引入 BetterGenshinImpact.GameTask.Common.TaskControl 命名空间中的静态成员

namespace BetterGenshinImpact.GameTask.AutoFight; // 定义命名空间 BetterGenshinImpact.GameTask.AutoFight

/// <summary>
/// 一键战斗宏
/// </summary>
public class OneKeyFightTask : Singleton<OneKeyFightTask> // 定义 OneKeyFightTask 类，继承 Singleton<OneKeyFightTask>
{
    public static readonly string HoldOnMode = "按住时重复"; // 定义按住时重复模式的静态常量
    public static readonly string TickMode = "触发"; // 定义触发模式的静态常量

    private Dictionary<string, List<CombatCommand>>? _avatarMacros; // 存储角色宏的字典
    private CancellationTokenSource? _cts = null; // 用于取消任务的标记
    private Task? _fightTask; // 执行战斗任务的异步任务

    private bool _isKeyDown = false; // 表示按键是否被按下
    private int activeMacroPriority = -1; // 当前激活的宏优先级
    private DateTime _lastUpdateTime = DateTime.MinValue; // 上次更新时间

    private CombatScenes? _currentCombatScenes; // 当前战斗场景

    public void KeyDown() // 按键按下事件处理
    {
        if (_isKeyDown || !IsEnabled()) // 如果按键已被按下或任务未启用
        {
            return; // 退出方法
        }
        _isKeyDown = true; // 设置按键状态为按下
        if (activeMacroPriority != TaskContext.Instance().Config.MacroConfig.CombatMacroPriority || IsAvatarMacrosEdited()) // 检查宏优先级是否改变或宏是否被编辑
        {
            activeMacroPriority = TaskContext.Instance().Config.MacroConfig.CombatMacroPriority; // 更新激活的宏优先级
            _avatarMacros = LoadAvatarMacros(); // 加载角色宏
            Logger.LogInformation("加载一键宏配置完成"); // 记录宏配置加载完成的日志
        }

        if (IsHoldOnMode()) // 如果是按住时重复模式
        {
            if (_cts == null || _cts.Token.IsCancellationRequested) // 如果取消标记为空或已被请求
            {
                _cts = new CancellationTokenSource(); // 创建新的取消标记源
                _fightTask = FightTask(_cts); // 初始化战斗任务
                if (!_fightTask.IsCompleted) // 如果战斗任务尚未完成
                {
                    _fightTask.Start(); // 启动战斗任务
                }
            }
        }
        else if (IsTickMode()) // 如果是触发模式
        {
            if (_cts == null || _cts.Token.IsCancellationRequested) // 如果取消标记为空或已被请求
            {
                _cts = new CancellationTokenSource(); // 创建新的取消标记源
                _fightTask = FightTask(_cts); // 初始化战斗任务
                if (!_fightTask.IsCompleted) // 如果战斗任务尚未完成
                {
                    _fightTask.Start(); // 启动战斗任务
                }
            }
            else
            {
                _cts.Cancel(); // 取消当前战斗任务
            }
        }
    }

    public void KeyUp() // 按键释放事件处理
    {
        _isKeyDown = false; // 设置按键状态为未按下
        if (!IsEnabled()) // 如果任务未启用
        {
            return; // 退出方法
        }

        if (IsHoldOnMode()) // 如果是按住时重复模式
        {
            _cts?.Cancel(); // 取消战斗任务
        }
    }

    // public void Run()
    // {
    //     if (!IsEnabled())
    //     {
    //         return;
    //     }
    //     _avatarMacros ??= LoadAvatarMacros();
    //
    //     if (IsHoldOnMode())
    //     {
    //         if (_fightTask == null || _fightTask.IsCompleted)
    //         {
    //             _fightTask = FightTask(_cts);
    //             _fightTask.Start();
    //         }
    //         Thread.Sleep(100); // 暂停当前线程100毫秒
    //     }
    //     else if (IsTickMode()) // 如果是 Tick 模式
    //     {
    //         if (_cts.Token.IsCancellationRequested) // 如果取消请求已发出
    //         {
    //             _cts = new CancellationTokenSource(); // 创建新的取消令牌源
    //             Task.Run(() => FightTask(_cts)); // 在新任务中运行 FightTask
    //         }
    //         else
    //         {
    //             _cts.Cancel(); // 取消当前的取消令牌源
    //         }
    //     }
    // }

    /// <summary>
    /// 循环执行战斗宏
    /// </summary>
    private Task FightTask(CancellationTokenSource cts)
    {
        // 切换截图模式
        var dispatcherCaptureMode = TaskTriggerDispatcher.Instance().GetCacheCaptureMode(); // 获取当前的截图模式
        if (dispatcherCaptureMode != DispatcherCaptureModeEnum.CacheCaptureWithTrigger) // 如果截图模式不是触发缓存模式
        {
            TaskTriggerDispatcher.Instance().SetCacheCaptureMode(DispatcherCaptureModeEnum.CacheCaptureWithTrigger); // 设置为触发缓存模式
            Sleep(TaskContext.Instance().Config.TriggerInterval * 2, cts); // 等待缓存图像，时间为触发间隔的两倍
        }

        var imageRegion = CaptureToRectArea(); // 捕获图像区域
        var combatScenes = new CombatScenes().InitializeTeam(imageRegion); // 初始化战斗场景和队伍
        if (!combatScenes.CheckTeamInitialized()) // 如果队伍初始化失败
        {
            if (_currentCombatScenes == null) // 如果当前没有战斗场景
            {
                Logger.LogError("首次队伍角色识别失败"); // 记录错误日志
                return Task.CompletedTask; // 完成任务
            }
            else
            {
                Logger.LogWarning("队伍角色识别失败，使用上次识别结果，队伍未切换时无影响"); // 记录警告日志
            }
        }
        else
        {
            _currentCombatScenes = combatScenes; // 更新当前战斗场景
        }
        // 找到出战角色
        var activeAvatar = _currentCombatScenes.Avatars.First(avatar => avatar.IsActive(imageRegion)); // 从当前战斗场景中找到活动角色

        if (_avatarMacros != null && _avatarMacros.TryGetValue(activeAvatar.Name, out var combatCommands)) // 如果存在角色宏且能够找到对应的宏命令
        {
            return new Task(() =>
            {
                Logger.LogInformation("→ {Name}执行宏", activeAvatar.Name); // 记录执行宏的日志
                while (!cts.Token.IsCancellationRequested && IsEnabled()) // 循环直到取消请求发出或功能被禁用
                {
                    if (IsHoldOnMode() && !_isKeyDown) // 如果是保持模式且没有按下键
                    {
                        break; // 退出循环
                    }

                    // 通用化战斗策略
                    foreach (var command in combatCommands) // 遍历战斗命令
                    {
                        command.Execute(activeAvatar); // 执行战斗命令
                    }
                }
                Logger.LogInformation("→ {Name}停止宏", activeAvatar.Name); // 记录停止宏的日志
            });
        }
        else
        {
            Logger.LogWarning("→ {Name}配置[{Priority}]为空，请先配置一键宏", activeAvatar.Name, activeMacroPriority); // 记录警告日志
            return Task.CompletedTask; // 完成任务
        }
    }

    public Dictionary<string, List<CombatCommand>> LoadAvatarMacros() // 定义一个方法用于加载角色宏
    # 获取 JSON 文件的绝对路径
    var jsonPath = Global.Absolute("User/avatar_macro.json");
    # 读取 JSON 文件的全部文本内容
    var json = File.ReadAllText(jsonPath);
    # 获取 JSON 文件的最后修改时间
    _lastUpdateTime = File.GetLastWriteTime(jsonPath);
    # 反序列化 JSON 内容为 AvatarMacro 对象列表
    var avatarMacros = JsonSerializer.Deserialize<List<AvatarMacro>>(json, ConfigService.JsonOptions);
    # 如果反序列化结果为空，返回空列表
    if (avatarMacros == null)
    {
        return [];
    }
    # 创建一个字典，键为 AvatarMacro 的名称，值为对应的 CombatCommand 列表
    var result = new Dictionary<string, List<CombatCommand>>();
    # 遍历 AvatarMacro 对象列表
    foreach (var avatarMacro in avatarMacros)
    {
        # 加载 AvatarMacro 对象的命令列表
        var commands = avatarMacro.LoadCommands();
        # 如果命令列表不为空，将其添加到结果字典中
        if (commands != null)
        {
            result.Add(avatarMacro.Name, commands);
        }
    }
    # 返回最终的结果字典
    return result;



    # 通过修改时间判断是否编辑过
    var jsonPath = Global.Absolute("User/avatar_macro.json");
    # 获取 JSON 文件的最后修改时间
    var lastWriteTime = File.GetLastWriteTime(jsonPath);
    # 如果文件的最后修改时间晚于上次更新的时间，返回 true，否则返回 false
    return lastWriteTime > _lastUpdateTime;



    # 检查宏配置是否启用
    return TaskContext.Instance().Config.MacroConfig.CombatMacroEnabled;



    # 检查是否处于 HoldOn 模式
    return TaskContext.Instance().Config.MacroConfig.CombatMacroHotkeyMode == HoldOnMode;



    # 检查是否处于 Tick 模式
    return TaskContext.Instance().Config.MacroConfig.CombatMacroHotkeyMode == TickMode;
能否提供更多代码上下文，以便我准确理解这一行的作用？
```