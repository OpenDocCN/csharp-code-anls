# `.\better-genshin-impact\BetterGenshinImpact\GameTask\AutoFight\AutoFightTask.cs`

```cs
# 导入 BetterGenshinImpact 游戏任务相关的库和命名空间
﻿using BetterGenshinImpact.GameTask.AutoFight.Assets;
using BetterGenshinImpact.GameTask.AutoFight.Model;
using BetterGenshinImpact.GameTask.AutoFight.Script;
using BetterGenshinImpact.GameTask.AutoGeniusInvokation.Exception;
using BetterGenshinImpact.GameTask.Model.Enum;
using BetterGenshinImpact.View.Drawable;
using BetterGenshinImpact.ViewModel.Pages;
using Microsoft.Extensions.Logging;
using System;
using System.Threading.Tasks;
using static BetterGenshinImpact.GameTask.Common.TaskControl;

# 定义 BetterGenshinImpact 游戏任务中的 AutoFight 命名空间
namespace BetterGenshinImpact.GameTask.AutoFight;

# 定义一个公共类 AutoFightTask
public class AutoFightTask
{
    # 声明只读字段，用于存储战斗任务参数
    private readonly AutoFightParam _taskParam;

    # 声明只读字段，用于存储战斗脚本包
    private readonly CombatScriptBag _combatScriptBag;

    # 构造函数，初始化战斗任务参数并解析战斗策略
    public AutoFightTask(AutoFightParam taskParam)
    {
        # 将传入的战斗任务参数赋值给字段
        _taskParam = taskParam;

        # 使用战斗策略路径读取并解析战斗脚本，赋值给字段
        _combatScriptBag = CombatScriptParser.ReadAndParse(_taskParam.CombatStrategyPath);
    }

    # 定义异步启动战斗任务的方法
    public async void Start()
    {
        // 初始化锁标志为 false
        var hasLock = false;
        try
        {
            // 销毁 AutoFightAssets 实例
            AutoFightAssets.DestroyInstance();
            // 尝试异步获取锁，0 表示立即返回
            hasLock = await TaskSemaphore.WaitAsync(0);
            // 如果未成功获取锁
            if (!hasLock)
            {
                // 记录错误信息并返回
                Logger.LogError("启动自动战斗功能失败：当前存在正在运行中的独立任务，请不要重复执行任务！");
                return;
            }

            // 初始化相关设置
            Init();
            // 创建 CombatScenes 实例并初始化队伍
            var combatScenes = new CombatScenes().InitializeTeam(CaptureToRectArea());
            // 检查队伍是否成功初始化
            if (!combatScenes.CheckTeamInitialized())
            {
                // 如果失败，抛出异常
                throw new Exception("识别队伍角色失败");
            }
            // 从 _combatScriptBag 中找到适用于当前队伍的战斗脚本
            var combatCommands = _combatScriptBag.FindCombatScript(combatScenes.Avatars);

            // 在任务开始之前进行处理
            combatScenes.BeforeTask(_taskParam.Cts);

            // 执行战斗操作
            await Task.Run(() =>
            {
                try
                {
                    // 当取消标志未被请求时循环
                    while (!_taskParam.Cts.Token.IsCancellationRequested)
                    {
                        // 执行所有战斗命令
                        foreach (var command in combatCommands)
                        {
                            command.Execute(combatScenes);
                        }
                    }
                }
                catch (NormalEndException)
                {
                    // 记录战斗操作结束信息
                    Logger.LogInformation("战斗操作结束");
                }
                catch (Exception e)
                {
                    // 记录警告信息并重新抛出异常
                    Logger.LogWarning(e.Message);
                    throw;
                }
            });
        }
        catch (NormalEndException)
        {
            // 记录手动中断信息
            Logger.LogInformation("手动中断自动战斗");
        }
        catch (Exception e)
        {
            // 记录错误信息和调试信息
            Logger.LogError(e.Message);
            Logger.LogDebug(e.StackTrace);
        }
        finally
        {
            // 清除 VisionContext 中的内容
            VisionContext.Instance().DrawContent.ClearAll();
            // 恢复正常触发模式
            TaskTriggerDispatcher.Instance().SetCacheCaptureMode(DispatcherCaptureModeEnum.NormalTrigger);
            // 设置自动战斗按钮文本
            TaskSettingsPageViewModel.SetSwitchAutoFightButtonText(false);
            // 记录自动战斗结束信息
            Logger.LogInformation("→ {Text}", "自动战斗结束");

            // 如果成功获取锁，则释放锁
            if (hasLock)
            {
                TaskSemaphore.Release();
            }
        }
    }

    private void Init()
    {
        // 记录屏幕分辨率
        LogScreenResolution();
        // 记录启动自动战斗的信息
        Logger.LogInformation("→ {Text}", "自动战斗，启动！");
        // 激活系统窗口
        SystemControl.ActivateWindow();
        // 设置缓存触发模式
        TaskTriggerDispatcher.Instance().SetCacheCaptureMode(DispatcherCaptureModeEnum.CacheCaptureWithTrigger);
        // 等待缓存图像
        Sleep(TaskContext.Instance().Config.TriggerInterval * 5, _taskParam.Cts);
    }

    private void LogScreenResolution()
    {
        // 获取游戏窗口的分辨率
        var gameScreenSize = SystemControl.GetGameScreenRect(TaskContext.Instance().GameHandle);
        // 检查分辨率是否为 16:9
        if (gameScreenSize.Width * 9 != gameScreenSize.Height * 16)
        {
            // 记录警告信息，指出分辨率问题
            Logger.LogWarning("游戏窗口分辨率不是 16:9 ！当前分辨率为 {Width}x{Height} , 非 16:9 分辨率的游戏可能无法正常使用自动战斗功能 !", gameScreenSize.Width, gameScreenSize.Height);
        }
    }
// 结束一个代码块或函数
}
```