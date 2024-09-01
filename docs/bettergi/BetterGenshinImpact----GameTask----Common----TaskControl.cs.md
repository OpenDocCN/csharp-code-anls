# `.\better-genshin-impact\BetterGenshinImpact\GameTask\Common\TaskControl.cs`

```cs
﻿using BetterGenshinImpact.GameTask.AutoGeniusInvokation.Exception;  // 引入异常处理类
using BetterGenshinImpact.GameTask.Model.Area;  // 引入区域模型类
using Fischless.GameCapture;  // 引入游戏捕捉相关类
using Microsoft.Extensions.Logging;  // 引入日志记录接口
using System;  // 引入基本系统类
using System.Drawing;  // 引入图形处理类
using System.Threading;  // 引入线程处理类
using System.Threading.Tasks;  // 引入异步任务处理类

namespace BetterGenshinImpact.GameTask.Common;  // 定义命名空间

public class TaskControl  // 定义 TaskControl 类
{
    public static ILogger Logger { get; } = App.GetLogger<TaskControl>();  // 静态属性，用于获取任务控制的日志记录器

    public static readonly SemaphoreSlim TaskSemaphore = new(1, 1);  // 静态只读字段，用于限制同时执行的任务数量

    public static void CheckAndSleep(int millisecondsTimeout)  // 公共静态方法，用于检查状态并休眠
    {
        if (!SystemControl.IsGenshinImpactActiveByProcess())  // 如果当前进程不是原神
        {
            Logger.LogInformation("当前获取焦点的窗口不是原神，停止执行");  // 记录信息日志
            throw new NormalEndException("当前获取焦点的窗口不是原神");  // 抛出正常结束异常
        }

        Thread.Sleep(millisecondsTimeout);  // 休眠指定的时间
    }

    public static void Sleep(int millisecondsTimeout)  // 公共静态方法，用于休眠
    {
        NewRetry.Do(() =>  // 执行带重试机制的操作
        {
            if (!SystemControl.IsGenshinImpactActiveByProcess())  // 如果当前进程不是原神
            {
                Logger.LogInformation("当前获取焦点的窗口不是原神，暂停");  // 记录信息日志
                throw new RetryException("当前获取焦点的窗口不是原神");  // 抛出重试异常
            }
        }, TimeSpan.FromSeconds(1), 100);  // 设置重试时间间隔和次数
        Thread.Sleep(millisecondsTimeout);  // 休眠指定的时间
    }

    public static void Sleep(int millisecondsTimeout, CancellationTokenSource? cts)  // 公共静态方法，带有取消令牌的休眠
    {
        if (cts is { IsCancellationRequested: true })  // 如果取消令牌请求了取消
        {
            throw new NormalEndException("取消自动任务");  // 抛出正常结束异常
        }

        if (millisecondsTimeout <= 0)  // 如果休眠时间小于或等于0
        {
            return;  // 直接返回
        }

        NewRetry.Do(() =>  // 执行带重试机制的操作
        {
            if (cts is { IsCancellationRequested: true })  // 如果取消令牌请求了取消
            {
                throw new NormalEndException("取消自动任务");  // 抛出正常结束异常
            }

            if (!SystemControl.IsGenshinImpactActiveByProcess())  // 如果当前进程不是原神
            {
                Logger.LogInformation("当前获取焦点的窗口不是原神，暂停");  // 记录信息日志
                throw new RetryException("当前获取焦点的窗口不是原神");  // 抛出重试异常
            }
        }, TimeSpan.FromSeconds(1), 100);  // 设置重试时间间隔和次数
        Thread.Sleep(millisecondsTimeout);  // 休眠指定的时间
        if (cts is { IsCancellationRequested: true })  // 再次检查取消令牌
        {
            throw new NormalEndException("取消自动任务");  // 抛出正常结束异常
        }
    }

    public static async Task Delay(int millisecondsTimeout, CancellationTokenSource cts)  // 公共静态异步方法，带有取消令牌的延迟
    {
        # 检查是否取消了任务，如果取消，则抛出 NormalEndException 异常，表示任务被取消
        if (cts is { IsCancellationRequested: true })
        {
            throw new NormalEndException("取消自动任务");
        }

        # 如果超时时间小于等于 0，则直接返回，不进行任何操作
        if (millisecondsTimeout <= 0)
        {
            return;
        }

        # 使用 NewRetry 类来进行重试操作
        NewRetry.Do(() =>
        {
            # 检查是否取消了任务，如果取消，则抛出 NormalEndException 异常，表示任务被取消
            if (cts is { IsCancellationRequested: true })
            {
                throw new NormalEndException("取消自动任务");
            }

            # 如果当前活动窗口不是原神，则记录信息并抛出 RetryException 异常
            if (!SystemControl.IsGenshinImpactActiveByProcess())
            {
                Logger.LogInformation("当前获取焦点的窗口不是原神，暂停");
                throw new RetryException("当前获取焦点的窗口不是原神");
            }
        }, TimeSpan.FromSeconds(1), 100); # 重试逻辑，每秒重试一次，总共重试 100 次

        # 异步等待指定的超时时间，如果任务被取消，则抛出异常
        await Task.Delay(millisecondsTimeout, cts.Token);
        if (cts is { IsCancellationRequested: true })
        {
            throw new NormalEndException("取消自动任务");
        }
    }

    public static Bitmap CaptureGameBitmap(IGameCapture? gameCapture)
    {
        # 尝试从 gameCapture 捕获截图
        var bitmap = gameCapture?.Capture();
        # 如果截图模式为 WindowsGraphicsCapture，则至少截图 3 次
        if (gameCapture?.Mode == CaptureModes.WindowsGraphicsCapture)
        {
            for (int i = 0; i < 2; i++)
            {
                bitmap = gameCapture?.Capture(); # 捕获截图
                Sleep(50); # 暂停 50 毫秒
            }
        }

        # 如果截图为空，记录警告信息并重试
        if (bitmap == null)
        {
            Logger.LogWarning("截图失败!");
            # 尝试重试 15 次
            for (var i = 0; i < 15; i++)
            {
                bitmap = gameCapture?.Capture(); # 捕获截图
                if (bitmap != null)
                {
                    return bitmap; # 如果成功捕获截图，返回截图
                }

                Sleep(30); # 暂停 30 毫秒
            }

            throw new Exception("尝试多次后,截图失败!"); # 如果所有尝试都失败，抛出异常
        }
        else
        {
            return bitmap; # 返回成功捕获的截图
        }
    }

    private static CaptureContent CaptureToContent(IGameCapture? gameCapture)
    {
        # 捕获游戏截图并创建 CaptureContent 对象
        var bitmap = CaptureGameBitmap(gameCapture);
        return new CaptureContent(bitmap, 0, 0);
    }

    // [Obsolete]
    // public static CaptureContent CaptureToContent()
    // {
    //     return CaptureToContent(TaskTriggerDispatcher.GlobalGameCapture);
    // }

    // public static ImageRegion CaptureToRectArea()
    // {
    //     return CaptureToContent(TaskTriggerDispatcher.GlobalGameCapture).CaptureRectArea;
    // }

    // /// <summary>
    // /// 此方法 TaskDispatcher至少处于 DispatcherCaptureModeEnum.CacheCaptureWithTrigger 状态才能使用
    // /// </summary>
    // /// <returns></returns>
    // [Obsolete]
    // public static CaptureContent GetContentFromDispatcher()
    // {
    //     return TaskTriggerDispatcher.Instance().GetLastCaptureContent();
    // }

    // /// <summary>
    // /// 此方法 TaskDispatcher至少处于 DispatcherCaptureModeEnum.CacheCaptureWithTrigger 状态才能使用
    // /// </summary>
    // /// <returns></returns>
    // public static ImageRegion GetRectAreaFromDispatcher()
    // {
    //     return TaskTriggerDispatcher.Instance().GetLastCaptureContent().CaptureRectArea;
    // }

    /// <summary>
    # 注释说明当前方法的作用是自动选择合适的截图方式并返回截图
    /// 自动判断当前运行上下文中截图方式，并选择合适的截图方式返回
    /// </summary>
    /// <returns></returns>
    // 这行注释是为文档生成工具自动生成方法的返回值说明
    public static ImageRegion CaptureToRectArea()
    {
        // 调用 TaskTriggerDispatcher 实例的方法 CaptureToRectArea 并返回其结果
        return TaskTriggerDispatcher.Instance().CaptureToRectArea();
    }
可以提供更多代码上下文吗？这样我能更好地理解代码的含义。
```