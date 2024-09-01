# `.\better-genshin-impact\BetterGenshinImpact\GameTask\AutoPathing\PathRecorder.cs`

```cs
# 使用命名空间和类库，设置路径记录器的相关功能
﻿using System;
using System.Collections.Generic;
using System.Text;
using BetterGenshinImpact.GameTask.Common;
using BetterGenshinImpact.GameTask.Common.Map;
using Microsoft.Extensions.Logging;
using System.Text.Json;
using BetterGenshinImpact.Core.Config;
using System.IO;
using BetterGenshinImpact.GameTask.AutoPathing.Model;
using BetterGenshinImpact.GameTask.Common.BgiVision;

namespace BetterGenshinImpact.GameTask.AutoPathing;

# 定义路径记录器类
public class PathRecorder
{
    # 路径记录任务属性，初始化为新建的路径任务
    public Model.PathingTask PathingTask { get; set; } = new Model.PathingTask();
    
    # 开始路径记录的方法
    public void Start()
    {
        # 记录日志信息，标记路径点记录的开始
        TaskControl.Logger.LogInformation("开始路径点记录");
        
        # 创建新的路径点对象
        var waypoint = new Model.Waypoint();
        
        # 捕获当前屏幕区域的图像
        var screen = TaskControl.CaptureToRectArea();
        
        # 从屏幕图像中获取当前位置
        var position = Navigation.GetPosition(screen);
        
        # 将位置从主2048坐标系转换为游戏坐标系
        position = MapCoordinate.Main2048ToGame(position);
        
        # 设置路径点的 X 坐标
        waypoint.X = position.X;
        
        # 设置路径点的 Y 坐标
        waypoint.Y = position.Y;
        
        # 设置路径点的类型为传送点
        waypoint.WaypointType = Model.WaypointType.Teleport;
        
        # 设置路径点的移动类型为步行
        waypoint.MoveType = Model.MoveType.Walk;
        
        # 将创建的路径点添加到路径任务中
        PathingTask.Waypoints.Add(waypoint);
        
        # 记录日志信息，标记初始路径点的创建
        TaskControl.Logger.LogInformation("已创建初始路径点({x},{y})", waypoint.X, waypoint.Y);
    }

    # 添加途径点的方法，默认途径点类型为中转点
    public void AddWaypoint(WaypointType waypointType = WaypointType.Transit)
    {
        # 创建新的路径点对象
        var waypoint = new Model.Waypoint();
        
        # 捕获当前屏幕区域的图像
        var screen = TaskControl.CaptureToRectArea();
        
        # 从屏幕图像中获取当前位置
        var position = Navigation.GetPosition(screen);
        
        # 将位置从主2048坐标系转换为游戏坐标系
        position = MapCoordinate.Main2048ToGame(position);
        
        # 设置途径点的 X 坐标
        waypoint.X = position.X;
        
        # 设置途径点的 Y 坐标
        waypoint.Y = position.Y;
        
        # 设置途径点的类型为传入的 waypointType 参数值
        waypoint.WaypointType = waypointType;
        
        # 获取当前屏幕图像的运动状态
        var motionStatus = Bv.GetMotionStatus(screen);
        
        # 根据运动状态设置途径点的移动类型
        switch (motionStatus)
        {
            case MotionStatus.Fly:
                # 如果状态为飞行，设置移动类型为飞行
                waypoint.MoveType = MoveType.Fly;
                break;
            case MotionStatus.Climb:
                # 如果状态为攀爬，设置移动类型为跳跃
                waypoint.MoveType = MoveType.Jump;
                break;
            default:
                # 默认情况下，设置移动类型为步行
                waypoint.MoveType = MoveType.Walk;
                break;
        }
        
        # 将创建的途径点添加到路径任务中
        PathingTask.Waypoints.Add(waypoint);
        
        # 记录日志信息，标记途径点的添加
        TaskControl.Logger.LogInformation("已添加途径点({x},{y})", waypoint.X, waypoint.Y);
    }

    # 保存路径任务的方法
    public void Save()
    {
        # 将路径任务对象序列化为 JSON 字符串
        var json = JsonSerializer.Serialize(PathingTask);
        
        # 将 JSON 字符串写入指定路径的文件中，文件名包含当前日期和时间
        File.WriteAllText(Global.Absolute($@"log\way\{DateTime.Now:yyyy-MM-dd HH：mm：ss：ffff}.json"), json);
    }

    # 清除当前路径任务的方法
    public void Clear() {
        # 重新初始化路径任务对象为新的路径任务
        PathingTask = new Model.PathingTask();
    }
}
```