# `.\better-genshin-impact\BetterGenshinImpact\GameTask\AutoPathing\Model\PathingTask.cs`

```cs
# 引入系统库
﻿using System;
# 引入泛型集合库
using System.Collections.Generic;
# 引入文本处理库
using System.Text;

# 定义命名空间
namespace BetterGenshinImpact.GameTask.AutoPathing.Model;

# 使类 PathingTask 可以被序列化
[Serializable]
public class PathingTask
{
    # PathingTask 的信息，可能为空
    public PathingTaskInfo? Info { get; set; }
    # 路径点列表，初始化为空列表
    public List<Waypoint> Waypoints { get; set; } = [];
}

# 使类 Waypoint 可以被序列化
[Serializable]
public class Waypoint
{
    # X 坐标
    public double X { get; set; }
    # Y 坐标
    public double Y { get; set; }
    # 路径点类型
    public WaypointType WaypointType { get; set; }
    # 移动类型
    public MoveType MoveType { get; set; }
    # 动作类型，可能为空
    public ActionType? ActionType { get; set; }
}

# 定义路径点类型枚举
public enum WaypointType
{
    # 过渡点
    Transit,
    # 目标点
    Target,
    # 传送点
    Teleport,
}

# 定义移动类型枚举
public enum MoveType
{
    # 行走
    Walk,
    # 飞行
    Fly,
    # 跳跃
    Jump,
    # 游泳
    Swim,
}

# 定义动作类型枚举
public enum ActionType
{
    # 停止飞行
    StopFlying,
}

# 定义路径任务类型枚举
public enum PathingTaskType
{
    # 采集
    Collect, 
    # 挖矿
    Mining, 
    # 锄地
    Fight 
}

# 使类 PathingTaskInfo 可以被序列化
[Serializable]
public class PathingTaskInfo
{
    # 任务名称，初始化为空字符串
    public string Name { get; set; } = string.Empty;
    # 任务描述，初始化为空字符串
    public string Description { get; set; } = string.Empty;

    # 任务类型，表示 PathingTaskType 类型
    /// <summary>
    /// 任务类型 PathingTaskType
    /// </summary>
    public string Type { get; set; } = string.Empty;

    # 任务参数/配置
    # 持续操作 切换某个角色 长E or 短E
    # 持续疾跑
    # 边跳边走
}
```