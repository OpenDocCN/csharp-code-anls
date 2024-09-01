# `.\better-genshin-impact\BetterGenshinImpact\GameTask\AutoPathing\PathExecutor.cs`

```cs
﻿using BetterGenshinImpact.GameTask.AutoPathing.Model; // 引入路径规划模型
using BetterGenshinImpact.GameTask.Common; // 引入通用功能
using BetterGenshinImpact.GameTask.Common.BgiVision; // 引入视觉处理功能
using System; // 引入基本系统功能
using System.Collections.Generic; // 引入集合功能
using System.Threading; // 引入线程控制功能
using System.Threading.Tasks; // 引入异步任务功能
using BetterGenshinImpact.GameTask; // 引入游戏任务功能
using System.Windows.Forms; // 引入 Windows 表单功能
using Vanara.PInvoke; // 引入 Win32 API 功能
using Microsoft.Extensions.Logging; // 引入日志记录功能
using System.Linq; // 引入 LINQ 扩展方法
using BetterGenshinImpact.GameTask.AutoTrackPath; // 引入自动追踪路径功能
using BetterGenshinImpact.GameTask.Common.Map; // 引入地图相关功能
using BetterGenshinImpact.Core.Simulator; // 引入模拟器功能
using OpenCvSharp; // 引入 OpenCV 功能
using Wpf.Ui.Violeta.Controls; // 引入 WPF 控件功能
using BetterGenshinImpact.GameTask.Model.Area; // 引入区域模型功能

namespace BetterGenshinImpact.GameTask.AutoPathing; // 定义命名空间

public class PathExecutor(CancellationTokenSource cts) // 定义路径执行器类，接收取消令牌源
{
    public async Task Pathing(PathingTask task) // 异步方法，执行路径规划任务
    {
        if (!TaskContext.Instance().IsInitialized) // 检查任务上下文是否已初始化
        {
            Toast.Warning("请先在启动页，启动截图器再使用本功能"); // 弹出警告提示
            return; // 退出方法
        }

        SystemControl.ActivateWindow(); // 激活系统窗口

        if (task.Waypoints.Count == 0) // 检查路径点列表是否为空
        {
            TaskControl.Logger.LogWarning("没有路径点，寻路结束"); // 记录警告日志
            return; // 退出方法
        }

        if (task.Waypoints.First().WaypointType != WaypointType.Teleport) // 检查第一个路径点是否为传送点
        {
            TaskControl.Logger.LogWarning("第一个路径点不是传送点，将不会进行传送"); // 记录警告日志
        }

        // 这里应该判断一下自动拾取是否处于工作状态，但好像没有什么方便的读取办法
        // TODO:大地图传送的时候使用游戏坐标，追踪的时候应该使用2048地图图像坐标，这里临时做转换，后续改进
        var waypoints = new List<Waypoint>(); // 创建新的路径点列表
        foreach (var waypoint in task.Waypoints) // 遍历每个路径点
        {
            if (waypoint.WaypointType == WaypointType.Teleport) // 如果路径点为传送点
            {
                waypoints.Add(waypoint); // 添加到新列表中
                continue; // 跳过后续代码，继续下一个路径点
            }
            var waypointCopy = new Waypoint // 创建路径点副本
            {
                ActionType = waypoint.ActionType, // 复制动作类型
                WaypointType = waypoint.WaypointType, // 复制路径点类型
                MoveType = waypoint.MoveType // 复制移动类型
            };
            (waypointCopy.X, waypointCopy.Y) = MapCoordinate.GameToMain2048(waypoint.X, waypoint.Y); // 转换坐标
            waypoints.Add(waypointCopy); // 添加到新列表中
        }

        foreach (var waypoint in waypoints) // 遍历每个路径点
        {
            if (waypoint.WaypointType == WaypointType.Teleport) // 如果路径点为传送点
            {
                TaskControl.Logger.LogInformation("正在传送到{x},{y}", waypoint.X, waypoint.Y); // 记录信息日志
                await new TpTask(cts).Tp(waypoint.X, waypoint.Y); // 执行传送任务
                continue; // 跳过后续代码，继续下一个路径点
            }

            // waypoint.WaypointType == WaypointType.Path 或者 WaypointType.Target
            // Path不用走得很近，Target需要接近，但都需要先移动到对应位置

            await MoveTo(waypoint); // 移动到路径点

            if (waypoint.WaypointType == WaypointType.Target) // 如果路径点为目标点
            {
                await MoveCloseTo(waypoint); // 移动到目标点附近
            }
        }
    }

    private async Task MoveTo(Waypoint waypoint) // 异步方法，移动到指定路径点
    }

    private async Task MoveCloseTo(Waypoint waypoint) // 异步方法，移动到指定路径点附近
    }
    # 捕获当前屏幕区域
    var screen = TaskControl.CaptureToRectArea();
    # 获取屏幕上当前位置
    var position = Navigation.GetPosition(screen);
    # 获取到目标方向
    var targetOrientation = Navigation.GetTargetOrientation(waypoint, position);
    # 记录精确接近路径点的信息
    TaskControl.Logger.LogInformation("精确接近路径点，当前位置({x1},{y1})，目标位置({x2},{y2})", position.X, position.Y, waypoint.X, waypoint.Y);
    # 检查当前是否在飞行状态
    var isFlying = Bv.GetMotionStatus(screen) == MotionStatus.Fly;
    # 如果目标是停止飞行且当前在飞行中，则执行点击操作
    if (waypoint.MoveType == MoveType.Fly && waypoint.ActionType == ActionType.StopFlying && isFlying)
    {
        # 下落攻击接近目的地
        Simulation.SendInput.Mouse.LeftButtonClick();
        await Task.Delay(1000);
    }
    # 等待直到旋转到目标方向
    await WaitUntilRotatedTo(targetOrientation, 2);
    var wPressed = false;
    var stepsTaken = 0;
    while (true)
    {
        stepsTaken++;
        # 如果步数超过8次，则记录超时警告并退出循环
        if (stepsTaken > 8)
        {
            TaskControl.Logger.LogWarning("精确接近超时");
            break;
        }
        # 捕获当前屏幕区域
        screen = TaskControl.CaptureToRectArea();
        # 获取当前位置
        position = Navigation.GetPosition(screen);
        # 如果已到达目标路径点，记录信息并退出循环
        if (Navigation.GetDistance(waypoint, position) < 2)
        {
            TaskControl.Logger.LogInformation("已到达路径点");
            break;
        }
        # 旋转到目标方向
        RotateTo(targetOrientation, screen); # 不再改变视角
        if (waypoint.MoveType == MoveType.Walk)
        {
            # 小碎步接近目标
            Simulation.SendInput.Keyboard.KeyPress(User32.VK.VK_W);
            await Task.Delay(500);
            continue;
        }
        if (!wPressed)
        {
            # 按下"W"键以开始移动
            Simulation.SendInput.Keyboard.KeyDown(User32.VK.VK_W);
        }
        await Task.Delay(500);
    }
    # 如果"W"键被按下，则释放它
    if (wPressed)
    {
        Simulation.SendInput.Keyboard.KeyUp(User32.VK.VK_W);
    }
    # 旋转到目标方向
    private int RotateTo(int targetOrientation, ImageRegion imageRegion, int controlRatio = 6)
    {
        # 计算当前相机的方向
        var cao = CameraOrientation.Compute(imageRegion.SrcGreyMat);
        # 计算与目标方向的差值
        var diff = (cao - targetOrientation + 180) % 360 - 180;
        diff += diff < -180 ? 360 : 0;
        if (diff == 0)
        {
            return diff;
        }
        # 根据差值移动鼠标以调整方向
        Simulation.SendInput.Mouse.MoveMouseBy(-controlRatio * (diff + (diff > 0 ? 1 : -1)), 0);
        return diff;
    }
    # 等待直到旋转到目标方向
    private async Task WaitUntilRotatedTo(int targetOrientation, int maxDiff)
    {
        int count = 0;
        while (true)
        {
            # 捕获当前屏幕区域
            var screen = TaskControl.CaptureToRectArea();
            # 检查当前方向是否在允许的误差范围内
            if (Math.Abs(RotateTo(targetOrientation, screen)) < maxDiff)
            {
                break;
            }
            # 如果检查次数超过50次则退出
            if (count > 50)
            {
                break;
            }
            await Task.Delay(50);
            count++;
        }
    }
能否提供更多的代码上下文或说明一下这段代码的位置？这有助于准确注释每一行。
```