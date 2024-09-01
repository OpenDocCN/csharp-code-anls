# `.\better-genshin-impact\BetterGenshinImpact\GameTask\AutoDomain\AutoDomainTask.cs`

```cs
# 引入 BetterGenshinImpact.Core.Config 命名空间中的类
﻿using BetterGenshinImpact.Core.Config;
# 引入 BetterGenshinImpact.Core.Recognition.OCR 命名空间中的类
using BetterGenshinImpact.Core.Recognition.OCR;
# 引入 BetterGenshinImpact.Core.Recognition.ONNX 命名空间中的类
using BetterGenshinImpact.Core.Recognition.ONNX;
# 引入 BetterGenshinImpact.Core.Simulator 命名空间中的类
using BetterGenshinImpact.Core.Simulator;
# 引入 BetterGenshinImpact.GameTask.AutoFight 命名空间中的类
using BetterGenshinImpact.GameTask.AutoFight;
# 引入 BetterGenshinImpact.GameTask.AutoFight.Assets 命名空间中的类
using BetterGenshinImpact.GameTask.AutoFight.Assets;
# 引入 BetterGenshinImpact.GameTask.AutoFight.Model 命名空间中的类
using BetterGenshinImpact.GameTask.AutoFight.Model;
# 引入 BetterGenshinImpact.GameTask.AutoFight.Script 命名空间中的类
using BetterGenshinImpact.GameTask.AutoFight.Script;
# 引入 BetterGenshinImpact.GameTask.AutoGeniusInvokation.Exception 命名空间中的类
using BetterGenshinImpact.GameTask.AutoGeniusInvokation.Exception;
# 引入 BetterGenshinImpact.GameTask.AutoPick.Assets 命名空间中的类
using BetterGenshinImpact.GameTask.AutoPick.Assets;
# 引入 BetterGenshinImpact.GameTask.Common.Map 命名空间中的类
using BetterGenshinImpact.GameTask.Common.Map;
# 引入 BetterGenshinImpact.GameTask.Model.Area 命名空间中的类
using BetterGenshinImpact.GameTask.Model.Area;
# 引入 BetterGenshinImpact.GameTask.Model.Enum 命名空间中的类
using BetterGenshinImpact.GameTask.Model.Enum;
# 引入 BetterGenshinImpact.Helpers 命名空间中的类
using BetterGenshinImpact.Helpers;
# 引入 BetterGenshinImpact.Service.Notification 命名空间中的类
using BetterGenshinImpact.Service.Notification;
# 引入 BetterGenshinImpact.View.Drawable 命名空间中的类
using BetterGenshinImpact.View.Drawable;
# 引入 BetterGenshinImpact.ViewModel.Pages 命名空间中的类
using BetterGenshinImpact.ViewModel.Pages;
# 引入 Compunet.YoloV8 命名空间中的类
using Compunet.YoloV8;
# 引入 Microsoft.Extensions.Logging 命名空间中的类
using Microsoft.Extensions.Logging;
# 引入 OpenCvSharp 命名空间中的类
using OpenCvSharp;
# 引入 System 命名空间中的类
using System;
# 引入 System.Collections.Generic 命名空间中的类
using System.Collections.Generic;
# 引入 System.Diagnostics 命名空间中的类
using System.Diagnostics;
# 引入 System.Drawing.Imaging 命名空间中的类
using System.Drawing.Imaging;
# 引入 System.IO 命名空间中的类
using System.IO;
# 引入 System.Threading 命名空间中的类
using System.Threading;
# 引入 System.Threading.Tasks 命名空间中的类
using System.Threading.Tasks;
# 引入 BetterGenshinImpact.GameTask.Common.TaskControl 命名空间中的静态成员
using static BetterGenshinImpact.GameTask.Common.TaskControl;
# 引入 Vanara.PInvoke.User32 命名空间中的静态成员
using static Vanara.PInvoke.User32;

# 定义命名空间 BetterGenshinImpact.GameTask.AutoDomain
namespace BetterGenshinImpact.GameTask.AutoDomain;

# 定义类 AutoDomainTask
public class AutoDomainTask
{
    # 定义只读字段，存储任务参数
    private readonly AutoDomainParam _taskParam;

    # 定义只读字段，存储模拟器实例
    private readonly PostMessageSimulator _simulator;

    # 定义只读字段，存储预测器实例
    private readonly YoloV8Predictor _predictor;

    # 定义只读字段，存储配置实例
    private readonly AutoDomainConfig _config;

    # 定义只读字段，存储战斗脚本集合
    private readonly CombatScriptBag _combatScriptBag;

    # 构造函数，初始化字段
    public AutoDomainTask(AutoDomainParam taskParam)
    {
        _taskParam = taskParam;
        # 从 AutoFightContext 实例中获取模拟器
        _simulator = AutoFightContext.Instance.Simulator;

        # 创建 YoloV8 预测器实例并配置 ONNX 模型
        _predictor = YoloV8Builder.CreateDefaultBuilder()
            .UseOnnxModel(Global.Absolute("Assets\\Model\\Domain\\bgi_tree.onnx"))
            .WithSessionOptions(BgiSessionOption.Instance.Options)
            .Build();

        # 从 TaskContext 实例中获取自动秘境配置
        _config = TaskContext.Instance().Config.AutoDomainConfig;

        # 解析并读取战斗策略文件
        _combatScriptBag = CombatScriptParser.ReadAndParse(_taskParam.CombatStrategyPath);
    }

    # 定义异步方法 Start，启动任务
    public async void Start()
    {
    }

    # 定义私有方法 Init，初始化任务
    private void Init()
    {
        # 记录屏幕分辨率
        LogScreenResolution();
        # 根据任务参数中的秘境轮次数输出启动信息
        if (_taskParam.DomainRoundNum == 9999)
        {
            Logger.LogInformation("→ {Text} 用尽所有体力后结束", "自动秘境，启动！");
        }
        else
        {
            Logger.LogInformation("→ {Text} 设置总次数：{Cnt}", "自动秘境，启动！", _taskParam.DomainRoundNum);
        }

        # 激活窗口
        SystemControl.ActivateWindow();
        # 设置任务触发器为仅缓存捕获模式
        TaskTriggerDispatcher.Instance().SetCacheCaptureMode(DispatcherCaptureModeEnum.OnlyCacheCapture);
        # 等待缓存图像
        Sleep(TaskContext.Instance().Config.TriggerInterval * 5, _taskParam.Cts);
    }

    # 定义私有方法 LogScreenResolution，记录屏幕分辨率
    private void LogScreenResolution()
    {
        // 获取游戏窗口的分辨率
        var gameScreenSize = SystemControl.GetGameScreenRect(TaskContext.Instance().GameHandle);
        // 判断分辨率是否为 16:9 比例
        if (gameScreenSize.Width * 9 != gameScreenSize.Height * 16)
        {
            // 如果不是 16:9 比例，记录警告日志
            Logger.LogWarning("游戏窗口分辨率不是 16:9 ！当前分辨率为 {Width}x{Height} , 非 16:9 分辨率的游戏可能无法正常使用自动秘境功能 !", gameScreenSize.Width, gameScreenSize.Height);
        }
        // 判断分辨率是否小于 1920x1080
        if (gameScreenSize.Width < 1920 || gameScreenSize.Height < 1080)
        {
            // 如果小于 1920x1080，记录警告日志
            Logger.LogWarning("游戏窗口分辨率小于 1920x1080 ！当前分辨率为 {Width}x{Height} , 小于 1920x1080 的分辨率的游戏可能无法正常使用自动秘境功能 !", gameScreenSize.Width, gameScreenSize.Height);
        }
    }

    private void RetryTeamInit(CombatScenes combatScenes)
    {
        // 检查队伍是否已初始化
        if (!combatScenes.CheckTeamInitialized())
        {
            // 如果未初始化，初始化队伍
            combatScenes.InitializeTeam(CaptureToRectArea());
            // 再次检查队伍是否已初始化
            if (!combatScenes.CheckTeamInitialized())
            {
                // 如果仍未初始化，抛出异常
                throw new Exception("识别队伍角色失败，请在较暗背景下重试，比如游戏时间调整成夜晚。或者直接使用强制指定当前队伍角色的功能。");
            }
        }
    }

    private void EnterDomain()
    {
        // 获取自动战斗上下文中的战斗资源
        var fightAssets = AutoFightContext.Instance.FightAssets;

        // 查找秘境入口区域并进行操作
        using var fRectArea = CaptureToRectArea().Find(AutoPickAssets.Instance.FRo);
        if (!fRectArea.IsEmpty())
        {
            // 按下键盘上的 F 键
            Simulation.SendInput.Keyboard.KeyPress(VK.VK_F);
            // 记录信息日志
            Logger.LogInformation("自动秘境：{Text}", "进入秘境");
            // 等待秘境开门动画完成，持续 5 秒
            Sleep(5000, _taskParam.Cts);
        }

        int retryTimes = 0, clickCount = 0;
        // 尝试点击确认按钮，最多重试 20 次，点击次数不超过 2 次
        while (retryTimes < 20 && clickCount < 2)
        {
            retryTimes++;
            using var confirmRectArea = CaptureToRectArea().Find(fightAssets.ConfirmRa);
            if (!confirmRectArea.IsEmpty())
            {
                // 点击确认按钮
                confirmRectArea.Click();
                clickCount++;
            }

            // 等待 1.5 秒再进行下一次尝试
            Sleep(1500, _taskParam.Cts);
        }

        // 等待载入动画完成，持续 3 秒
        Sleep(3000, _taskParam.Cts);
    }

    private void CloseDomainTip()
    {
        // 允许最多 2 分钟的时间来关闭提示
        var retryTimes = 0;
        while (retryTimes < 120)
        {
            retryTimes++;
            using var cactRectArea = CaptureToRectArea().Find(AutoFightContext.Instance.FightAssets.ClickAnyCloseTipRa);
            if (!cactRectArea.IsEmpty())
            {
                // 等待 1 秒，再点击关闭提示
                Sleep(1000, _taskParam.Cts);
                cactRectArea.Click();
                break;
            }

            // todo 添加小地图角标位置检测 防止有人手点了
            // 如果未找到，等待 1 秒后重试
            Sleep(1000, _taskParam.Cts);
        }

        // 额外等待 1.5 秒以确保关闭提示完成
        Sleep(1500, _taskParam.Cts);
    }

    private List<CombatCommand> FindCombatScriptAndSwitchAvatar(CombatScenes combatScenes)
    {
        // 查找战斗脚本
        var combatCommands = _combatScriptBag.FindCombatScript(combatScenes.Avatars);
        // 选择并切换到指定头像
        var avatar = combatScenes.SelectAvatar(combatCommands[0].Name);
        avatar?.SwitchWithoutCts();
        // 等待 200 毫秒
        Sleep(200);
        return combatCommands;
    }

    /// <summary>
    /// 走到钥匙处启动
    /// </summary>
    private async Task WalkToPressF()
    {
        # 如果取消请求已被发出，则退出当前方法
        if (_taskParam.Cts.Token.IsCancellationRequested)
        {
            return;
        }

        # 异步运行以下任务
        await Task.Run((Action)(() =>
        {
            # 模拟按下 W 键
            _simulator.KeyDown(VK.VK_W);
            # 暂停 20 毫秒
            Sleep(20);
            # 如果配置中 WalkToF 为 false，按下 SHIFT 键
            if (!_config.WalkToF)
            {
                Simulation.SendInput.Keyboard.KeyDown(VK.VK_SHIFT);
            }

            try
            {
                # 循环直到取消请求被发出
                while (!_taskParam.Cts.Token.IsCancellationRequested)
                {
                    # 捕获屏幕区域并查找特定资产
                    using var fRectArea = Common.TaskControl.CaptureToRectArea().Find(AutoPickAssets.Instance.FRo);
                    # 如果没有找到特定资产，等待 100 毫秒
                    if (fRectArea.IsEmpty())
                    {
                        Sleep(100, _taskParam.Cts);
                    }
                    else
                    {
                        # 记录检测到交互键的信息
                        Logger.LogInformation("检测到交互键");
                        # 模拟按下 F 键
                        Simulation.SendInput.Keyboard.KeyPress(VK.VK_F);
                        # 跳出循环
                        break;
                    }
                }
            }
            finally
            {
                # 释放 W 键
                _simulator.KeyUp(VK.VK_W);
                # 暂停 50 毫秒
                Sleep(50);
                # 如果配置中 WalkToF 为 false，释放 SHIFT 键
                if (!_config.WalkToF)
                {
                    Simulation.SendInput.Keyboard.KeyUp(VK.VK_SHIFT);
                }
            }
        }));
    }

    # 启动战斗任务
    private Task StartFight(CombatScenes combatScenes, List<CombatCommand> combatCommands)
    {
        # 创建取消令牌源
        CancellationTokenSource cts = new();
        # 注册取消令牌源的取消方法
        _taskParam.Cts.Token.Register(cts.Cancel);
        # 执行战斗场景中的任务前操作
        combatScenes.BeforeTask(cts);
        # 创建一个新的任务来执行战斗操作
        var combatTask = new Task(() =>
        {
            try
            {
                # 循环直到取消请求被发出
                while (!cts.Token.IsCancellationRequested)
                {
                    # 遍历战斗命令并执行
                    foreach (var command in combatCommands)
                    {
                        command.Execute(combatScenes);
                    }
                }
            }
            catch (NormalEndException e)
            {
                # 记录战斗操作中断的信息
                Logger.LogInformation("战斗操作中断：{Msg}", e.Message);
            }
            catch (Exception e)
            {
                # 记录警告信息并抛出异常
                Logger.LogWarning(e.Message);
                throw;
            }
            finally
            {
                # 记录自动战斗线程结束的信息
                Logger.LogInformation("自动战斗线程结束");
            }
        }, cts.Token);

        # 创建对局结束检测任务
        var domainEndTask = DomainEndDetectionTask(cts);

        # 启动战斗任务和对局结束检测任务
        combatTask.Start();
        domainEndTask.Start();

        # 返回两个任务的完成任务
        return Task.WhenAll(combatTask, domainEndTask);
    }

    # 等待战斗结束
    private void EndFightWait()
    {
        # 如果取消请求已被发出，则退出当前方法
        if (_taskParam.Cts.Token.IsCancellationRequested)
        {
            return;
        }

        # 获取战斗结束后的等待时间
        var s = TaskContext.Instance().Config.AutoDomainConfig.FightEndDelay;
        # 如果等待时间大于 0 秒，记录等待信息并暂停
        if (s > 0)
        {
            Logger.LogInformation("战斗结束后等待 {Second} 秒", s);
            Sleep((int)(s * 1000), _taskParam.Cts);
        }
    }

    /// <summary>
    /// 对局结束检测
    /// </summary>
    private Task DomainEndDetectionTask(CancellationTokenSource cts)
    {
        // 创建一个新的任务来检测领域结束
        return new Task(() =>
        {
            // 任务内部循环，直到任务参数的取消令牌被请求取消
            while (!_taskParam.Cts.Token.IsCancellationRequested)
            {
                // 检测是否领域结束
                if (IsDomainEnd())
                {
                    // 如果检测到领域结束，取消传入的取消令牌
                    cts.Cancel();
                    // 退出循环
                    break;
                }

                // 休眠 1000 毫秒
                Sleep(1000);
            }
        });
    }

    private bool IsDomainEnd()
    {
        // 创建用于捕获区域的资源
        using var ra = CaptureToRectArea();

        // 从捕获区域中裁剪出“挑战”提示区域
        var endTipsRect = ra.DeriveCrop(AutoFightContext.Instance.FightAssets.EndTipsUpperRect);
        // 对裁剪区域进行 OCR 识别
        var text = OcrFactory.Paddle.Ocr(endTipsRect.SrcGreyMat);
        // 如果识别到“挑战”或“达成”，则返回领域结束
        if (text.Contains("挑战") || text.Contains("达成"))
        {
            Logger.LogInformation("检测到秘境结束提示(挑战达成)，结束秘境");
            return true;
        }

        // 从捕获区域中裁剪出另一个“自动”退出提示区域
        endTipsRect = ra.DeriveCrop(AutoFightContext.Instance.FightAssets.EndTipsRect);
        // 对裁剪区域进行 OCR 识别
        text = OcrFactory.Paddle.Ocr(endTipsRect.SrcGreyMat);
        // 如果识别到“自动”或“退出”，则返回领域结束
        if (text.Contains("自动") || text.Contains("退出"))
        {
            Logger.LogInformation("检测到秘境结束提示(xxx秒后自动退出)，结束秘境");
            return true;
        }

        // 如果以上条件都不满足，返回领域未结束
        return false;
    }

    /// <summary>
    /// 旋转视角后寻找石化古树
    /// </summary>
    private Task FindPetrifiedTree()
    {
        // 创建一个取消令牌源，用于控制寻找石化古树任务
        CancellationTokenSource treeCts = new();
        // 将任务参数的取消令牌与新的取消令牌源关联
        _taskParam.Cts.Token.Register(treeCts.Cancel);
        // 中键点击鼠标来回正视角
        Simulation.SendInput.Mouse.MiddleButtonClick();
        // 休眠 900 毫秒，等待视角调整
        Sleep(900, _taskParam.Cts);

        // 创建任务以水平移动角色，直到石化古树位于屏幕中心
        var moveAvatarTask = MoveAvatarHorizontallyTask(treeCts);

        // 创建任务以锁定相机到东方向
        var lockCameraToEastTask = LockCameraToEastTask(treeCts, moveAvatarTask);
        // 启动锁定相机任务
        lockCameraToEastTask.Start();
        // 等待两个任务同时完成
        return Task.WhenAll(moveAvatarTask, lockCameraToEastTask);
    }

    private Task MoveAvatarHorizontallyTask(CancellationTokenSource treeCts)
    }

    private Rect DetectTree(ImageRegion region)
    {
        // 创建一个内存流以存储图像
        using var memoryStream = new MemoryStream();
        // 将图像保存到内存流中
        region.SrcBitmap.Save(memoryStream, ImageFormat.Bmp);
        // 将内存流的读取位置重新设置到起始位置
        memoryStream.Seek(0, SeekOrigin.Begin);
        // 使用预测器检测图像中的物体
        var result = _predictor.Detect(memoryStream);
        var list = new List<RectDrawable>();
        // 遍历检测结果中的每个框
        foreach (var box in result.Boxes)
        {
            // 创建一个矩形对象来表示检测框
            var rect = new Rect(box.Bounds.X, box.Bounds.Y, box.Bounds.Width, box.Bounds.Height);
            // 将矩形对象转换为可绘制的矩形，并添加到列表中
            list.Add(region.ToRectDrawable(rect, "tree"));
        }

        // 将检测到的矩形列表添加到视觉上下文中
        VisionContext.Instance().DrawContent.PutOrRemoveRectList("TreeBox", list);

        // 如果检测到至少一个矩形
        if (list.Count > 0)
        {
            // 返回第一个检测到的矩形
            var box = result.Boxes[0];
            return new Rect(box.Bounds.X, box.Bounds.Y, box.Bounds.Width, box.Bounds.Height);
        }

        // 如果未检测到任何矩形，返回空矩形
        return Rect.Empty;
    }

    private Task LockCameraToEastTask(CancellationTokenSource cts, Task moveAvatarTask)
    {
        // 返回一个新的任务
        return new Task(() =>
        {
            // 初始化连续东方向次数为 0
            var continuousCount = 0; // 连续东方向次数
            // 初始化开始标志为 false
            var started = false;
            // 循环直到取消请求被触发
            while (!cts.Token.IsCancellationRequested)
            {
                // 捕获当前视图区域
                using var captureRegion = CaptureToRectArea();
                // 计算当前视角的角度
                var angle = CameraOrientation.Compute(captureRegion.SrcGreyMat);
                // 在视图区域绘制方向
                CameraOrientation.DrawDirection(captureRegion, angle);
                if (angle is >= 356 or <= 4)
                {
                    // 角度在东方向范围内
                    continuousCount++;
                }

                if (angle < 180)
                {
                    // 如果角度小于 180，则视角左移
                    var moveAngle = angle;
                    if (moveAngle > 2)
                    {
                        moveAngle *= 2;
                    }

                    // 移动鼠标左移
                    Simulation.SendInput.Mouse.MoveMouseBy(-moveAngle, 0);
                    // 重置连续次数
                    continuousCount = 0;
                }
                else if (angle is > 180 and < 360)
                {
                    // 如果角度大于 180 且小于 360，则视角右移
                    var moveAngle = 360 - angle;
                    if (moveAngle > 2)
                    {
                        moveAngle *= 2;
                    }

                    // 移动鼠标右移
                    Simulation.SendInput.Mouse.MoveMouseBy(moveAngle, 0);
                    // 重置连续次数
                    continuousCount = 0;
                }
                else
                {
                    // 视角达到 360 度，即东方向视角
                    if (continuousCount > 5)
                    {
                        // 如果连续次数大于 5 并且任务尚未开始
                        if (!started && moveAvatarTask.Status != TaskStatus.Running)
                        {
                            // 标记任务已开始，并启动任务
                            started = true;
                            moveAvatarTask.Start();
                        }
                    }
                }

                // 休眠 100 毫秒
                Sleep(100);
            }

            // 记录线程结束信息
            Logger.LogInformation("锁定东方向视角线程结束");
            // 清除视觉内容
            VisionContext.Instance().DrawContent.ClearAll();
        });
    }

    /// <summary>
    /// 领取奖励
    /// </summary>
    /// <param name="recognizeResin">是否识别树脂</param>
    /// <param name="isLastTurn">是否最后一轮</param>
    private bool GettingTreasure(bool recognizeResin, bool isLastTurn)
    {
        // 等待 1500 毫秒以确保窗口弹出
        Sleep(1500, _taskParam.Cts);

        // 初始化重试次数为 0
        var retryTimes = 0;
        while (true)
        {
            // 增加重试次数
            retryTimes++;
            if (retryTimes > 3)
            {
                // 如果重试次数超过 3 次，记录信息并退出循环
                Logger.LogInformation("没有浓缩树脂了");
                break;
            }

            // 捕获区域并查找浓缩树脂的区域
            var useCondensedResinRa = CaptureToRectArea().Find(AutoFightContext.Instance.FightAssets.UseCondensedResinRa);
            if (!useCondensedResinRa.IsEmpty())
            {
                // 如果找到浓缩树脂区域，点击它
                useCondensedResinRa.Click();
                // 等待 400 毫秒，以解决水龙王按下左键后未松开的问题
                Sleep(400, _taskParam.Cts);
                // 再次点击浓缩树脂区域
                useCondensedResinRa.Click();
                break;
            }

            // 如果未找到浓缩树脂区域，等待 800 毫秒后重试
            Sleep(800, _taskParam.Cts);
        }

        // 等待 1000 毫秒以确保操作完成
        Sleep(1000, _taskParam.Cts);

        // 获取捕获区域
        var captureArea = TaskContext.Instance().SystemInfo.CaptureAreaRect;
        for (var i = 0; i < 30; i++)
        {
            // 跳过领取动画，通过点击区域来跳过
            GameCaptureRegion.GameRegionClick((size, scale) => (size.Width - 140 * scale, 53 * scale));
            Sleep(200, _taskParam.Cts);
            GameCaptureRegion.GameRegionClick((size, scale) => (size.Width - 140 * scale, 53 * scale));

            // 优先点击继续按钮
            var ra = CaptureToRectArea();
            var confirmRectArea = ra.Find(AutoFightContext.Instance.FightAssets.ConfirmRa);
            if (!confirmRectArea.IsEmpty())
            {
                if (isLastTurn)
                {
                    // 如果是最后一回合，查找并点击退出按钮
                    var exitRectArea = ra.Find(AutoFightContext.Instance.FightAssets.ExitRa);
                    if (!exitRectArea.IsEmpty())
                    {
                        exitRectArea.Click();
                        return false;
                    }
                }

                if (!recognizeResin)
                {
                    // 如果不需要识别树脂，点击确认按钮并返回 true
                    confirmRectArea.Click();
                    return true;
                }

                // 获取剩余树脂的状态
                var (condensedResinCount, fragileResinCount) = GetRemainResinStatus();
                if (condensedResinCount == 0 && fragileResinCount < 20)
                {
                    // 如果没有体力，查找并点击退出按钮
                    var exitRectArea = ra.Find(AutoFightContext.Instance.FightAssets.ExitRa);
                    if (!exitRectArea.IsEmpty())
                    {
                        exitRectArea.Click();
                        return false;
                    }
                }
                else
                {
                    // 如果有体力，点击确认按钮并返回 true
                    confirmRectArea.Click();
                    return true;
                }
            }

            // 如果未找到确认按钮，等待 300 毫秒后重试
            Sleep(300, _taskParam.Cts);
        }

        // 如果未检测到秘境结束，抛出异常提示可能是背包物品已满
        throw new NormalEndException("未检测到秘境结束，可能是背包物品已满。");
    }

    /// <summary>
    /// 获取剩余树脂状态
    /// </summary>
    private (int, int) GetRemainResinStatus()
    {
        // 初始化浓缩树脂和脆弱树脂的计数为 0
        var condensedResinCount = 0;
        var fragileResinCount = 0;

        // 捕捉当前矩形区域的图像
        var ra = CaptureToRectArea();
        // 查找浓缩树脂的区域
        var condensedResinCountRa = ra.Find(AutoFightContext.Instance.FightAssets.CondensedResinCountRa);
        // 如果找到浓缩树脂区域
        if (!condensedResinCountRa.IsEmpty())
        {
            // 计算并提取浓缩树脂数量区域的图像
            var countArea = ra.DeriveCrop(condensedResinCountRa.X + condensedResinCountRa.Width, condensedResinCountRa.Y, condensedResinCountRa.Width, condensedResinCountRa.Height);
            // 将图像保存到文件（注释掉的代码）
            // Cv2.ImWrite($"log/resin_{DateTime.Now.ToString("yyyy-MM-dd HH：mm：ss：ffff")}.png", countArea.SrcGreyMat);
            // 使用 OCR 技术识别提取的图像中的浓缩树脂数量
            var count = OcrFactory.Paddle.OcrWithoutDetector(countArea.SrcGreyMat);
            // 将识别到的数量转换为整数
            condensedResinCount = StringUtils.TryParseInt(count);
        }

        // 查找脆弱树脂的区域
        var fragileResinCountRa = ra.Find(AutoFightContext.Instance.FightAssets.FragileResinCountRa);
        // 如果找到脆弱树脂区域
        if (!fragileResinCountRa.IsEmpty())
        {
            // 计算并提取脆弱树脂数量区域的图像
            var countArea = ra.DeriveCrop(fragileResinCountRa.X + fragileResinCountRa.Width, fragileResinCountRa.Y, (int)(fragileResinCountRa.Width * 3), fragileResinCountRa.Height);
            // 使用 OCR 技术识别提取的图像中的脆弱树脂数量
            var count = OcrFactory.Paddle.Ocr(countArea.SrcGreyMat);
            // 将识别到的数量转换为整数
            fragileResinCount = StringUtils.TryParseInt(count);
        }

        // 记录浓缩树脂和脆弱树脂的剩余数量
        Logger.LogInformation("剩余：浓缩树脂 {CondensedResinCount} 脆弱树脂 {FragileResinCount}", condensedResinCount, fragileResinCount);
        // 返回浓缩树脂和脆弱树脂的数量
        return (condensedResinCount, fragileResinCount);
    }
请提供具体的代码片段或示例，以便我可以为每个语句添加详细注释。
```