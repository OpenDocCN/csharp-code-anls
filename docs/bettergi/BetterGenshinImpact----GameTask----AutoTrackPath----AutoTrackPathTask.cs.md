# `.\better-genshin-impact\BetterGenshinImpact\GameTask\AutoTrackPath\AutoTrackPathTask.cs`

```cs
# 导入所需的命名空间
﻿using BetterGenshinImpact.Core.Config;
using BetterGenshinImpact.Core.Simulator;
using BetterGenshinImpact.GameTask.AutoGeniusInvokation.Exception;
using BetterGenshinImpact.GameTask.AutoTrackPath.Model;
using BetterGenshinImpact.GameTask.Common;
using BetterGenshinImpact.GameTask.Common.BgiVision;
using BetterGenshinImpact.GameTask.Common.Element.Assets;
using BetterGenshinImpact.GameTask.Common.Map;
using BetterGenshinImpact.GameTask.Model.Area;
using BetterGenshinImpact.GameTask.Model.Enum;
using BetterGenshinImpact.Helpers;
using BetterGenshinImpact.Helpers.Extensions;
using BetterGenshinImpact.Service;
using BetterGenshinImpact.View.Drawable;
using BetterGenshinImpact.ViewModel.Pages;
using Microsoft.Extensions.Logging;
using OpenCvSharp;
using System;
using System.Collections.Generic;
using System.Diagnostics;
using System.Drawing;
using System.IO;
using System.Text.Json;
using System.Threading;
using System.Threading.Tasks;
using Vanara.PInvoke;
using static BetterGenshinImpact.GameTask.Common.TaskControl;

namespace BetterGenshinImpact.GameTask.AutoTrackPath;

# 类定义，包含自动追踪路径的任务逻辑
public class AutoTrackPathTask
{
    # 定义一个只读字段，存储任务参数
    private readonly AutoTrackPathParam _taskParam;

    # 定义一个路径对象
    private GiPath _way;

    # 视角偏移的移动单位常量
    private const int CharMovingUnit = 500;

    # 构造函数，初始化任务参数并从 JSON 文件加载路径数据
    public AutoTrackPathTask(AutoTrackPathParam taskParam)
    {
        _taskParam = taskParam;

        # 从文件读取路径 JSON 数据并反序列化成 GiPath 对象
        var wayJson = File.ReadAllText(Global.Absolute(@"log\way\way2.json"));
        _way = JsonSerializer.Deserialize<GiPath>(wayJson, ConfigService.JsonOptions) ?? throw new Exception("way json deserialize failed");
    }

    # 异步启动任务
    public async void Start()
    {
        var hasLock = false;
        try
        {
            # 尝试获取任务锁
            hasLock = await TaskSemaphore.WaitAsync(0);
            if (!hasLock)
            {
                # 锁获取失败时记录错误日志
                Logger.LogError("启动自动路线功能失败：当前存在正在运行中的独立任务，请不要重复执行任务！");
                return;
            }

            # 初始化任务设置
            Init();

            # 执行任务
            await DoTask();
        }
        catch (NormalEndException)
        {
            # 任务被手动中断时记录信息
            Logger.LogInformation("手动中断自动路线");
        }
        catch (Exception e)
        {
            # 记录任务执行过程中发生的异常信息
            Logger.LogError(e.Message);
            Logger.LogDebug(e.StackTrace);
        }
        finally
        {
            # 清除视图中的内容
            VisionContext.Instance().DrawContent.ClearAll();
            # 设置任务触发模式为正常触发
            TaskTriggerDispatcher.Instance().SetCacheCaptureMode(DispatcherCaptureModeEnum.NormalTrigger);
            # 设置自动战斗按钮文本为“关闭”
            TaskSettingsPageViewModel.SetSwitchAutoFightButtonText(false);
            # 记录任务结束日志
            Logger.LogInformation("→ {Text}", "自动路线结束");

            # 释放任务锁
            if (hasLock)
            {
                TaskSemaphore.Release();
            }
        }
    }

    # 初始化任务设置的私有方法
    private void Init()
    {
        // 激活系统窗口
        SystemControl.ActivateWindow();
        // 记录信息日志，表示自动路线已启动
        Logger.LogInformation("→ {Text}", "自动路线，启动！");
        // 设置任务触发器为仅缓存捕获模式
        TaskTriggerDispatcher.Instance().SetCacheCaptureMode(DispatcherCaptureModeEnum.OnlyCacheCapture);
        // 等待指定时间，期间根据任务上下文配置进行图像缓存
        Sleep(TaskContext.Instance().Config.TriggerInterval * 5, _taskParam.Cts); // 等待缓存图像
    }

    public void Stop()
    {
        // 取消任务的取消标记
        _taskParam.Cts.Cancel();
    }

    public async Task DoTask()
    {
        // 1. 传送到最近的传送点
        var first = _way.WayPointList[0]; // 解析路线，第一个点为起点
        await new TpTask(_taskParam.Cts).Tp(first.Pt.X, first.Pt.Y);

        // 2. 等待传送完成
        Sleep(1000);
        // 尝试捕获图像区域，并检查传送是否完成
        NewRetry.Do((Action)(() =>
        {
            var ra = TaskControl.CaptureToRectArea();
            var miniMapMat = GetMiniMapMat(ra) ?? throw new RetryException("等待传送完成");
        }), TimeSpan.FromSeconds(1), 100);
        // 记录传送完成的信息日志
        Logger.LogInformation("传送完成");
        Sleep(1000);

        // 3. 横向移动偏移量校准，移动指定偏移、按下W后识别朝向
        var angleOffset = GetOffsetAngle();
        if (angleOffset == 0)
        {
            throw new InvalidOperationException("横向移动偏移量校准失败");
        }

        // 4. 针对点位进行直线追踪

        // 创建任务取消源
        var trackCts = new CancellationTokenSource();
        _taskParam.Cts.Token.Register(trackCts.Cancel);
        // 启动直线追踪任务
        var trackTask = Track(_way.WayPointList, angleOffset, trackCts);
        trackTask.Start();
        // 启动刷新状态任务
        var refreshStatusTask = RefreshStatus(trackCts);
        refreshStatusTask.Start();
        // 启动跳跃任务
        var jumpTask = Jump(trackCts);
        jumpTask.Start();
        // 等待所有任务完成
        await Task.WhenAll(trackTask, refreshStatusTask, jumpTask);
    }

    private MotionStatus _motionStatus = MotionStatus.Normal;

    public Task Jump(CancellationTokenSource trackCts)
    {
        return new Task(() =>
        {
            // 持续执行直到任务被取消或追踪任务被取消
            while (!_taskParam.Cts.Token.IsCancellationRequested && !trackCts.Token.IsCancellationRequested)
            {
                // 如果运动状态正常，执行跳跃动作
                if (_motionStatus == MotionStatus.Normal)
                {
                    MovementControl.Instance.SpacePress();
                    Sleep(300);
                    // 如果运动状态仍正常，再次执行跳跃动作
                    if (_motionStatus == MotionStatus.Normal)
                    {
                        MovementControl.Instance.SpacePress();
                        Sleep(3500);
                    }
                    else
                    {
                        Sleep(1600);
                    }
                }
                else
                {
                    Sleep(1600);
                }
            }
        });
    }

    private double _targetAngle = 0;

    public Task RefreshStatus(CancellationTokenSource trackCts)
    {
        // 返回一个新的 Task 对象，执行以下匿名方法
        return new Task(() =>
        {
            // 当任务参数的取消令牌和跟踪取消令牌都未请求取消时，循环执行
            while (!_taskParam.Cts.Token.IsCancellationRequested && !trackCts.Token.IsCancellationRequested)
            {
                // 捕获当前区域的图像
                using var ra = CaptureToRectArea();
                // 获取捕获区域的运动状态
                _motionStatus = Bv.GetMotionStatus(ra);

                // 注释掉的代码可能用于获取迷你地图矩阵并计算角度，画出方向
                // var miniMapMat = GetMiniMapMat(ra);
                // if (miniMapMat == null)
                // {
                //     throw new InvalidOperationException("当前不在主界面");
                // }
                //
                // var angle = CharacterOrientation.Compute(miniMapMat);
                // CameraOrientation.DrawDirection(ra, angle, "avatar", new Pen(Color.Blue, 1));
                // Debug.WriteLine($"当前人物图像坐标系角度：{angle}");

                // 注释掉的代码可能用于计算移动角度并进行鼠标移动
                // var moveAngle = (int)(_targetAngle - angle);
                // Debug.WriteLine($"旋转到目标角度：{_targetAngle}，鼠标平移{moveAngle}单位");
                // Simulation.SendInput.Mouse.MoveMouseBy(moveAngle, 0);
                // 暂停60毫秒
                Sleep(60);
            }
        });
    }

    // 定义一个公开的方法，返回 Task 对象，接受点列表、角度偏移和取消令牌源作为参数
    public Task Track(List<GiPathPoint> pList, int angleOffsetUnit, CancellationTokenSource trackCts)
    }

    /// <summary>
    ///  地图图像点位
    ///  寻找后面20个点位中，下一个最近点位，关键点必须走到
    /// </summary>
    /// <param name="currPoint">当前点</param>
    /// <param name="pList">点列表</param>
    /// <param name="currIndex">当前索引</param>
    /// <returns>返回最近的点和其索引</returns>
    public (GiPathPoint, int) GetNextPoint(Point2f currPoint, List<GiPathPoint> pList, int currIndex)
    {
        // 计算搜索的点范围，最多搜索20个点
        var nextNum = Math.Min(currIndex + 20, pList.Count - 1);
        var minDistance = double.MaxValue;
        var minDistancePoint = pList[currIndex];
        var minDistanceIndex = currIndex;
        // 注释掉的代码可能用于处理额外条件的最小距离
        // var minDistanceButGt = double.MaxValue;
        // var minDistancePointButGt = pList[currIndex];
        // var minDistanceIndexButGt = currIndex;
        // 遍历点列表中的点
        for (var i = currIndex; i < nextNum; i++)
        {
            var nextPoint = pList[i + 1];
            var nextMapImagePos = nextPoint.MatchPt;
            // 计算当前点与下一个点之间的距离
            var distance = MathHelper.Distance(nextMapImagePos, currPoint);
            if (distance < minDistance)
            {
                minDistance = distance;
                minDistancePoint = nextPoint;
                minDistanceIndex = i + 1;
                // 注释掉的代码可能用于处理额外条件的最小距离
                // if (distance > 5)
                // {
                //     minDistanceButGt = distance;
                //     minDistancePointButGt = nextPoint;
                //     minDistanceIndexButGt = i;
                // }
            }

            // 如果找到关键点，退出循环
            if (GiPathPoint.IsKeyPoint(nextPoint))
            {
                break;
            }
        }

        // 注释掉的代码可能用于处理额外条件的返回值
        // return minDistanceButGt >= double.MaxValue ? (minDistancePointButGt, minDistanceIndexButGt) : (minDistancePoint, minDistanceIndex);
        // 返回最小距离点和索引
        return (minDistancePoint, minDistanceIndex);
    }

    // 定义一个公开的方法，返回整数类型的角度偏移量
    public int GetOffsetAngle()
    # 记录角色初始方向角度
    var angle1 = GetCharacterOrientationAngle();
    # 模拟鼠标在水平方向移动特定单位
    Simulation.SendInput.Mouse.MoveMouseBy(CharMovingUnit, 0);
    # 暂停500毫秒，等待移动完成
    Sleep(500);
    # 模拟按下W键，等待100毫秒后松开W键
    Simulation.SendInput.Keyboard.KeyDown(User32.VK.VK_W).Sleep(100).KeyUp(User32.VK.VK_W);
    # 暂停1000毫秒，等待动作完成
    Sleep(1000);
    # 记录角色在操作后的方向角度
    var angle2 = GetCharacterOrientationAngle();
    # 计算角度偏移量
    var angleOffset = angle2 - angle1;
    # 记录日志，包含鼠标平移单位和角度偏移量
    Logger.LogInformation("横向移动偏移量校准：鼠标平移{CharMovingUnit}单位，角度转动{AngleOffset}", CharMovingUnit, angleOffset);
    # 返回计算得到的角度偏移量
    return angleOffset;



    # 从图像区域ra中找到“Paimon”菜单图标
    public Mat? GetMiniMapMat(ImageRegion ra)
    {
        var paimon = ra.Find(ElementAssets.Instance.PaimonMenuRo);
        # 如果找到“Paimon”菜单图标
        if (paimon.IsExist())
        {
            # 从图像区域中提取出迷你地图区域的矩阵
            return new Mat(ra.SrcMat, new Rect(paimon.X + 24, paimon.Y - 15, 210, 210));
        }
        # 如果没有找到图标，返回null
        return null;
    }



    # 获取角色当前方向角度
    public int GetCharacterOrientationAngle()
    {
        # 捕获当前屏幕的矩形区域
        var ra = CaptureToRectArea();
        # 从矩形区域中获取迷你地图矩阵，如果获取失败则抛出异常
        var miniMapMat = GetMiniMapMat(ra) ?? throw new InvalidOperationException("当前不在主界面");
        # 计算并获取角色方向角度
        var angle = CharacterOrientation.Compute(miniMapMat);
        # 记录日志，包含当前角度
        Logger.LogInformation("当前角度：{Angle}", angle);
        # 取消注释下面这行代码以绘制方向图示
        # CameraOrientation.DrawDirection(ra, angle);
        # 返回计算得到的角度
        return angle;
    }
可以请你提供更多的代码上下文吗？这样我可以更准确地为每个语句添加注释。
```