# `.\better-genshin-impact\BetterGenshinImpact\GameTask\AutoSkip\AutoTrackTask.cs`

```cs
﻿using BetterGenshinImpact.Core.Recognition; // 引入核心识别功能相关的命名空间
using BetterGenshinImpact.Core.Recognition.OCR; // 引入OCR识别相关的命名空间
using BetterGenshinImpact.Core.Recognition.OpenCv; // 引入OpenCV识别相关的命名空间
using BetterGenshinImpact.Core.Simulator; // 引入模拟器相关的命名空间
using BetterGenshinImpact.GameTask.AutoGeniusInvokation.Exception; // 引入自动天才召唤相关异常的命名空间
using BetterGenshinImpact.GameTask.AutoSkip.Model; // 引入自动跳过功能模型相关的命名空间
using BetterGenshinImpact.GameTask.Common; // 引入游戏任务通用功能的命名空间
using BetterGenshinImpact.GameTask.Common.BgiVision; // 引入BgiVision相关功能的命名空间
using BetterGenshinImpact.GameTask.Common.Element.Assets; // 引入通用元素资产相关的命名空间
using BetterGenshinImpact.GameTask.Model; // 引入游戏任务模型相关的命名空间
using BetterGenshinImpact.GameTask.Model.Area; // 引入游戏任务区域模型相关的命名空间
using BetterGenshinImpact.GameTask.QuickTeleport.Assets; // 引入快速传送资产相关的命名空间
using BetterGenshinImpact.Helpers; // 引入辅助功能的命名空间
using BetterGenshinImpact.View.Drawable; // 引入可绘制视图的命名空间
using BetterGenshinImpact.ViewModel.Pages; // 引入视图模型页面的命名空间
using Microsoft.Extensions.Logging; // 引入Microsoft日志扩展的命名空间
using OpenCvSharp; // 引入OpenCV的Sharp封装库的命名空间
using System; // 引入系统功能的命名空间
using System.Collections.Generic; // 引入集合类的命名空间
using System.Diagnostics; // 引入调试相关的命名空间
using System.Linq; // 引入LINQ查询功能的命名空间
using Vanara.PInvoke; // 引入Vanara PInvoke功能的命名空间
using static BetterGenshinImpact.GameTask.Common.TaskControl; // 静态引入任务控制相关功能

namespace BetterGenshinImpact.GameTask.AutoSkip; // 定义命名空间 BetterGenshinImpact.GameTask.AutoSkip

public class AutoTrackTask(AutoTrackParam param) : BaseIndependentTask // 定义 AutoTrackTask 类，继承自 BaseIndependentTask
{
    // /// <summary>
    // /// 准备好前进了
    // /// </summary>
    // private bool _readyMoveForward = false; // 声明一个布尔变量，用于指示是否准备好前进，暂时被注释掉

    /// <summary>
    /// 任务距离
    /// </summary>
    private Rect _missionDistanceRect = Rect.Empty; // 声明一个 Rect 类型的变量用于表示任务距离，初始值为 Empty

    public async void Start() // 定义一个异步方法 Start，用于启动自动追踪任务
    {
        var hasLock = false; // 声明一个布尔变量，用于指示是否获得了锁
        try
        {
            hasLock = await TaskSemaphore.WaitAsync(0); // 尝试异步获取锁，如果获取失败，hasLock 设为 false
            if (!hasLock) // 如果没有获取到锁
            {
                Logger.LogError("启动自动追踪功能失败：当前存在正在运行中的独立任务，请不要重复执行任务！"); // 记录启动失败的错误日志
                return; // 退出 Start 方法
            }

            SystemControl.ActivateWindow(); // 激活系统窗口

            Logger.LogInformation("→ {Text}", "自动追踪，启动！"); // 记录启动自动追踪的日志信息

            TrackMission(); // 调用 TrackMission 方法开始追踪任务
        }
        catch (NormalEndException e) // 捕获 NormalEndException 异常
        {
            Logger.LogInformation("自动追踪中断:" + e.Message); // 记录自动追踪中断的日志信息
            // NotificationHelper.SendTaskNotificationWithScreenshotUsing(b => b.Domain().Cancelled().Build()); // 发送任务中断通知，注释掉
        }
        catch (Exception e) // 捕获所有其他异常
        {
            Logger.LogError(e.Message); // 记录错误日志信息
            Logger.LogDebug(e.StackTrace); // 记录调试日志信息，显示堆栈跟踪
            // NotificationHelper.SendTaskNotificationWithScreenshotUsing(b => b.Domain().Failure().Build()); // 发送任务失败通知，注释掉
        }
        finally
        {
            VisionContext.Instance().DrawContent.ClearAll(); // 清除视图内容
            TaskSettingsPageViewModel.SetSwitchAutoTrackButtonText(false); // 设置自动追踪按钮的文本为 false
            Logger.LogInformation("→ {Text}", "自动追踪结束"); // 记录自动追踪结束的日志信息

            if (hasLock) // 如果获得了锁
            {
                TaskSemaphore.Release(); // 释放锁
            }
        }
    }

    private void TrackMission() // 定义 TrackMission 方法，暂未实现
    {
    }

    private void StartTrackPoint() // 定义 StartTrackPoint 方法，暂未实现
    {
    }
}
    {
        // 模拟按下 V 键
        Simulation.SendInput.Keyboard.KeyPress(User32.VK.VK_V);
        // 等待 3 秒钟，参数使用 param.Cts
        Sleep(3000, param.Cts);

        // 捕捉当前区域到矩形区域
        var ra = CaptureToRectArea();
        // 在捕捉到的区域中查找蓝色追踪点
        var blueTrackPointRa = ra.Find(ElementAssets.Instance.BlueTrackPoint);
        // 如果找到蓝色追踪点
        if (blueTrackPointRa.IsExist())
        {
            // 将蓝色追踪点调整到正上方
            MakeBlueTrackPointDirectlyAbove();
        }
        else
        {
            // 如果首次未找到追踪点，记录警告日志
            Logger.LogWarning("首次未找到追踪点");
        }
    }

    /// <summary>
    /// 找到追踪点并调整方向
    /// </summary>
    private void MakeBlueTrackPointDirectlyAbove()
    {
        // 返回一个新的任务，该任务包含以下代码块
        // {
        // 初始化先前移动的 X 坐标为 0
        int prevMoveX = 0;
        // 初始化标志，表示 W 键是否按下
        bool wDown = false;
        // 当取消请求未被发出时，持续循环
        while (!param.Cts.Token.IsCancellationRequested)
        {
            // 捕获当前区域并存储
            var ra = CaptureToRectArea();
            // 在捕获的区域中查找蓝色追踪点
            var blueTrackPointRa = ra.Find(ElementAssets.Instance.BlueTrackPoint);
            // 如果找到蓝色追踪点
            if (blueTrackPointRa.IsExist())
            {
                // 使追踪点位于俯视角的上方
                var centerY = blueTrackPointRa.Y + blueTrackPointRa.Height / 2;
                // 如果追踪点的中心 Y 坐标高于捕获区域的中点
                if (centerY > CaptureRect.Height / 2)
                {
                    // 移动鼠标向左
                    Simulation.SendInput.Mouse.MoveMouseBy(-50, 0);
                    // 如果 W 键被按下，则释放 W 键
                    if (wDown)
                    {
                        Simulation.SendInput.Keyboard.KeyUp(User32.VK.VK_W);
                        wDown = false;
                    }
                    // 打印日志信息
                    Debug.WriteLine("使追踪点位于俯视角上方");
                    continue;
                }

                // 调整方向
                var centerX = blueTrackPointRa.X + blueTrackPointRa.Width / 2;
                var moveX = (centerX - CaptureRect.Width / 2) / 8;
                // 对移动量进行限制
                moveX = moveX switch
                {
                    > 0 and < 10 => 10,
                    > -10 and < 0 => -10,
                    _ => moveX
                };
                // 如果移动量不为 0，则移动鼠标
                if (moveX != 0)
                {
                    Simulation.SendInput.Mouse.MoveMouseBy(moveX, 0);
                    Debug.WriteLine("调整方向:" + moveX);
                }

                // 如果移动量为 0 或者移动方向与上一个移动方向相反
                if (moveX == 0 || prevMoveX * moveX < 0)
                {
                    // 如果 W 键未被按下，则按下 W 键
                    if (!wDown)
                    {
                        Simulation.SendInput.Keyboard.KeyDown(User32.VK.VK_W);
                        wDown = true;
                    }
                }

                // 如果移动量小于 50 且追踪点的中心 Y 坐标接近捕获区域的中点
                if (Math.Abs(moveX) < 50 && Math.Abs(centerY - CaptureRect.Height / 2) < 200)
                {
                    // 如果 W 键被按下，则释放 W 键
                    if (wDown)
                    {
                        Simulation.SendInput.Keyboard.KeyUp(User32.VK.VK_W);
                        wDown = false;
                    }
                    // 识别距离
                    var text = OcrFactory.Paddle.OcrWithoutDetector(ra.SrcGreyMat[_missionDistanceRect]);
                    // 如果识别结果为 1 到 3 的正整数
                    if (StringUtils.TryExtractPositiveInt(text) is > -1 and <= 3)
                    {
                        Logger.LogInformation("任务追踪：到达目标,识别结果[{Text}]", text);
                        break;
                    }
                    Logger.LogInformation("任务追踪：到达目标");
                    break;
                }

                // 更新上一个移动的 X 坐标
                prevMoveX = moveX;
            }
            else
            {
                // 如果未找到追踪点，则记录日志信息
                Logger.LogInformation("未找到追踪点");
            }

            // 移动鼠标向下以保证俯视角
            Simulation.SendInput.Mouse.MoveMouseBy(0, 500);
            // 休眠 100 毫秒
            Sleep(100);
        }
        // });
    }

    // 从任务文本中获取距离的方法
    private int GetDistanceFromMissionText(List<Region> textRaList)
    {
        // 将所有任务文本拼接成一个字符串，以“|”分隔
        var text = textRaList.Aggregate(string.Empty, (current, textRa) => current + textRa.Text.Trim() + "|");
        // 记录任务追踪日志，输出拼接后的任务文本
        Logger.LogInformation("任务追踪：{Text}", text);

        // 遍历文本列表
        foreach (var textRa in textRaList)
        {
            // 如果文本长度小于 8 且包含字母 'm'（忽略大小写）
            if (textRa.Text.Length < 8 && textRa.Text.Contains('m', StringComparison.OrdinalIgnoreCase))
            {
                // 将文本位置转换为游戏捕捉区域
                _missionDistanceRect = textRa.ConvertSelfPositionToGameCaptureRegion();
                // 尝试从文本中提取正整数并返回
                return StringUtils.TryExtractPositiveInt(textRa.Text);
            }
        }

        // 如果没有找到符合条件的文本，则返回 -1
        return -1;
    }

    // 私有方法：根据给定区域获取 OCR 任务文本列表
    private List<Region> OcrMissionTextRaList(Region paimonMenuRa)
    {
        // 在指定区域内进行多重识别操作，返回识别到的区域列表
        return CaptureToRectArea().FindMulti(new RecognitionObject
        {
            // 设置识别类型为 OCR
            RecognitionType = RecognitionTypes.Ocr,
            // 设置识别区域
            RegionOfInterest = new Rect(paimonMenuRa.X, paimonMenuRa.Y - 15 + 210,
                (int)(300 * AssetScale), (int)(100 * AssetScale))
        });
    }
# 结束一个代码块或代码段，具体含义取决于上下文
}
```