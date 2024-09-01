# `.\better-genshin-impact\BetterGenshinImpact\GameTask\TaskRunner.cs`

```cs
﻿using BetterGenshinImpact.Core.Script; // 引用 BetterGenshinImpact.Core.Script 命名空间
using BetterGenshinImpact.GameTask.AutoGeniusInvokation.Exception; // 引用 BetterGenshinImpact.GameTask.AutoGeniusInvokation.Exception 命名空间
using BetterGenshinImpact.GameTask.Model.Enum; // 引用 BetterGenshinImpact.GameTask.Model.Enum 命名空间
using BetterGenshinImpact.View; // 引用 BetterGenshinImpact.View 命名空间
using BetterGenshinImpact.View.Drawable; // 引用 BetterGenshinImpact.View.Drawable 命名空间
using Microsoft.Extensions.Logging; // 引用 Microsoft.Extensions.Logging 命名空间
using System; // 引用 System 命名空间
using System.Threading; // 引用 System.Threading 命名空间
using System.Threading.Tasks; // 引用 System.Threading.Tasks 命名空间
using static BetterGenshinImpact.GameTask.Common.TaskControl; // 引用 BetterGenshinImpact.GameTask.Common.TaskControl 命名空间中的静态成员

namespace BetterGenshinImpact.GameTask; // 定义命名空间 BetterGenshinImpact.GameTask

/// <summary>
/// 用于以独立任务的方式执行任意方法
/// </summary>
public class TaskRunner
{
    private readonly ILogger<TaskRunner> _logger = App.GetLogger<TaskRunner>(); // 初始化用于记录日志的 ILogger 实例

    private readonly DispatcherTimerOperationEnum _timerOperation = DispatcherTimerOperationEnum.None; // 初始化定时器操作的枚举，默认为 None

    private readonly string _name = string.Empty; // 初始化任务名称，默认为空字符串

    public TaskRunner() // 默认构造函数
    {
    }

    public TaskRunner(DispatcherTimerOperationEnum timerOperation) // 带参数的构造函数，用于初始化定时器操作
    {
        _timerOperation = timerOperation; // 设置定时器操作
    }

    /// <summary>
    /// 加锁并独立运行任务
    /// </summary>
    /// <param name="action"></param>
    /// <returns></returns>
    public async Task RunAsync(Func<Task> action) // 异步方法，用于运行传入的任务
    {
        // 加锁
        var hasLock = await TaskSemaphore.WaitAsync(0); // 尝试获取锁，等待超时为 0，返回锁是否获取成功
        if (!hasLock) // 如果未获取到锁
        {
            _logger.LogError("任务启动失败：当前存在正在运行中的独立任务，请不要重复执行任务！"); // 记录错误日志，提示任务启动失败
            return; // 退出方法
        }

        try
        {
            _logger.LogInformation("→ {Text}", _name + "任务启动！"); // 记录任务启动信息

            // 初始化
            Init(); // 调用初始化方法

            // 发送运行任务通知
            SendNotification(); // 发送任务运行通知

            CancellationContext.Instance.Set(); // 设置取消上下文

            await action(); // 执行传入的任务
        }
        catch (NormalEndException e) // 捕获 NormalEndException 异常
        {
            _logger.LogInformation("任务中断:{Msg}", e.Message); // 记录任务中断信息
            SendNotification(); // 发送任务中断通知
        }
        catch (Exception e) // 捕获所有其他异常
        {
            _logger.LogError(e.Message); // 记录错误信息
            _logger.LogDebug(e.StackTrace); // 记录错误堆栈跟踪
            SendNotification(); // 发送任务失败通知
        }
        finally
        {
            End(); // 调用结束方法
            _logger.LogInformation("→ {Text}", _name + "任务结束"); // 记录任务结束信息
            SendNotification(); // 发送任务结束通知

            CancellationContext.Instance.Clear(); // 清除取消上下文

            // 释放锁
            if (hasLock) // 如果之前获取了锁
            {
                TaskSemaphore.Release(); // 释放锁
            }
        }
    }

    public void FireAndForget(Func<Task> action) // 不等待任务完成直接执行的方法
    {
        Task.Run(() => RunAsync(action)); // 在新任务中异步运行指定的任务
    }

    public void Init() // 初始化方法
    {
        // 激活原神窗口
        var maskWindow = MaskWindow.Instance(); // 获取 MaskWindow 的实例
        SystemControl.ActivateWindow(); // 激活当前窗口
        maskWindow.Invoke(maskWindow.Show); // 调用 maskWindow 的 Show 方法以确保窗口可见
        if (_timerOperation == DispatcherTimerOperationEnum.UseSelfCaptureImage)
        {
            Thread.Sleep(TaskContext.Instance().Config.TriggerInterval * 5); // 等待日志窗口被激活
            TaskTriggerDispatcher.Instance().SetCacheCaptureMode(DispatcherCaptureModeEnum.Stop); // 设置缓存捕获模式为停止
        }
        else if (_timerOperation == DispatcherTimerOperationEnum.UseCacheImage)
        {
            TaskTriggerDispatcher.Instance().SetCacheCaptureMode(DispatcherCaptureModeEnum.OnlyCacheCapture); // 设置缓存捕获模式为仅缓存捕获
            Thread.Sleep(TaskContext.Instance().Config.TriggerInterval * 5); // 等待缓存图像
        }
        else if (_timerOperation == DispatcherTimerOperationEnum.UseCacheImageWithTrigger)
        {
            TaskTriggerDispatcher.Instance().SetCacheCaptureMode(DispatcherCaptureModeEnum.CacheCaptureWithTrigger); // 设置缓存捕获模式为缓存捕获带触发器
            Thread.Sleep(TaskContext.Instance().Config.TriggerInterval * 5); // 等待缓存图像
        }
        else if (_timerOperation == DispatcherTimerOperationEnum.UseCacheImageWithTriggerEmpty)
        {
            TaskTriggerDispatcher.Instance().SetCacheCaptureMode(DispatcherCaptureModeEnum.CacheCaptureWithTrigger); // 设置缓存捕获模式为缓存捕获带触发器
            TaskTriggerDispatcher.Instance().ClearTriggers(); // 清除所有触发器
            Thread.Sleep(TaskContext.Instance().Config.TriggerInterval * 5); // 等待缓存图像
        }
    }

    public void End()
    {
        VisionContext.Instance().DrawContent.ClearAll(); // 清除所有绘制内容
        if (_timerOperation == DispatcherTimerOperationEnum.UseSelfCaptureImage)
        {
            TaskTriggerDispatcher.Instance().SetCacheCaptureMode(DispatcherCaptureModeEnum.Start); // 重新开始缓存捕获模式
        }
        else if (_timerOperation is DispatcherTimerOperationEnum.UseCacheImage or DispatcherTimerOperationEnum.UseCacheImageWithTrigger or DispatcherTimerOperationEnum.UseCacheImageWithTriggerEmpty)
        {
            // 还原到原来的模式
            if (TaskContext.Instance().Config.CommonConfig.ScreenshotEnabled || TaskContext.Instance().Config.MacroConfig.CombatMacroEnabled)
            {
                TaskTriggerDispatcher.Instance().SetCacheCaptureMode(DispatcherCaptureModeEnum.CacheCaptureWithTrigger); // 根据配置还原缓存捕获模式为带触发器的缓存捕获
            }
            else
            {
                TaskTriggerDispatcher.Instance().SetCacheCaptureMode(DispatcherCaptureModeEnum.NormalTrigger); // 根据配置还原缓存捕获模式为普通触发器
            }

            if (_timerOperation == DispatcherTimerOperationEnum.UseCacheImageWithTriggerEmpty)
            {
                TaskTriggerDispatcher.Instance().SetTriggers(GameTaskManager.LoadInitialTriggers()); // 重新设置初始触发器
            }
        }
    }

    public void SendNotification()
    {
    }
请提供需要注释的代码。
```