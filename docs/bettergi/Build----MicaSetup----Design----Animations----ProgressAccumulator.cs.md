# `.\better-genshin-impact\Build\MicaSetup\Design\Animations\ProgressAccumulator.cs`

```cs
﻿using CommunityToolkit.Mvvm.ComponentModel;  // 引入 CommunityToolkit.Mvvm.ComponentModel 命名空间
using System;  // 引入 System 命名空间
using System.Threading;  // 引入 System.Threading 命名空间
using System.Threading.Tasks;  // 引入 System.Threading.Tasks 命名空间

namespace MicaSetup.Controls.Animations;  // 定义命名空间

public partial class ProgressAccumulator : ObservableObject, IDisposable  // 定义一个部分类 ProgressAccumulator，继承自 ObservableObject 和 IDisposable
{
    public Action<double>? Handler;  // 声明一个可选的 Action 委托，用于处理进度更新
    public DoubleEasingAnimation? Animation;  // 声明一个可选的 DoubleEasingAnimation 对象，用于动画效果

    [ObservableProperty]  // 标记字段为 ObservableProperty，以便生成通知属性更改的代码
    private double duration = 3000d;  // 声明一个私有字段，表示动画的持续时间，默认为 3000 毫秒

    [ObservableProperty]  // 标记字段为 ObservableProperty，以便生成通知属性更改的代码
    private double current = 0d;  // 声明一个私有字段，表示当前进度，默认为 0

    [ObservableProperty]  // 标记字段为 ObservableProperty，以便生成通知属性更改的代码
    private double from = 0d;  // 声明一个私有字段，表示进度起始值，默认为 0

    [ObservableProperty]  // 标记字段为 ObservableProperty，以便生成通知属性更改的代码
    private double to = 100d;  // 声明一个私有字段，表示进度结束值，默认为 100

    private Task task = null!;  // 声明一个私有字段，表示处理进度更新的任务，初始化时为 null
    private bool isRunning = false;  // 声明一个私有字段，表示进度累加器是否正在运行，默认为 false
    private DateTime startTime = default;  // 声明一个私有字段，表示进度开始时间，默认为默认值
    private double durationTime = default;  // 声明一个私有字段，表示经过的时间，默认为默认值

    public ProgressAccumulator(double from = 0d, double to = 100d, double duration = 3000d, Action<double> handler = null!, DoubleEasingAnimation anime = null!)  // 构造函数，初始化 ProgressAccumulator 对象
    {
        Reset(from, to, duration, handler);  // 调用 Reset 方法，初始化属性
        Animation = anime;  // 设置 Animation 属性
    }

    public void Dispose()  // 实现 IDisposable 接口的 Dispose 方法
    {
        Reset();  // 调用 Reset 方法，重置进度累加器
    }

    public ProgressAccumulator Start()  // 启动进度累加器
    {
        isRunning = true;  // 将 isRunning 设置为 true，表示开始运行
        startTime = DateTime.Now;  // 记录当前时间作为开始时间
        durationTime = default;  // 重置 durationTime
        task = Task.Run(Handle);  // 启动一个新任务，运行 Handle 方法
        return this;  // 返回当前对象，以支持链式调用
    }

    public ProgressAccumulator Stop()  // 停止进度累加器
    {
        isRunning = false;  // 将 isRunning 设置为 false，表示停止运行
        return this;  // 返回当前对象，以支持链式调用
    }

    public ProgressAccumulator Reset(double from = 0d, double to = 100d, double duration = 3000d, Action<double> handler = null!)  // 重置进度累加器
    {
        Stop();  // 停止进度累加器

        Current = From = from;  // 设置 Current 和 From 属性
        To = to;  // 设置 To 属性
        Duration = duration;  // 设置 Duration 属性
        Handler = handler;  // 设置 Handler 属性
        return this;  // 返回当前对象，以支持链式调用
    }

    private void Handle()  // 处理进度更新的方法
    {
        while (isRunning)  // 当 isRunning 为 true 时循环
        {
            if (!SpinWait.SpinUntil(() => !isRunning, 50))  // 等待 50 毫秒，直到 isRunning 为 false
            {
                Calc();  // 调用 Calc 方法计算当前进度
                Handler?.Invoke(Current);  // 如果 Handler 不为 null，则调用 Handler 处理当前进度
            }
        }
    }

    private double Calc()  // 计算进度的方法
    {
        if (durationTime <= Duration)  // 如果经过的时间小于或等于动画持续时间
        {
            Current = (Animation ?? DoubleEasingAnimations.EaseOutCirc).Invoke(durationTime, From, To, Duration);  // 计算当前进度
            durationTime = DateTime.Now.Subtract(startTime).TotalMilliseconds;  // 更新经过的时间
        }
        return Current;  // 返回当前进度
    }
}
```