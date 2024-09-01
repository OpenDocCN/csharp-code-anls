# `.\better-genshin-impact\BetterGenshinImpact\GameTask\AutoGeniusInvokation\Model\Duel.cs`

```cs
﻿using BetterGenshinImpact.Core.Recognition.OpenCv; // 引入 OpenCV 相关的命名空间
using BetterGenshinImpact.GameTask.AutoGeniusInvokation.Assets; // 引入游戏任务自动天赋召唤资源相关的命名空间
using BetterGenshinImpact.GameTask.AutoGeniusInvokation.Exception; // 引入游戏任务自动天赋召唤异常处理相关的命名空间
using BetterGenshinImpact.GameTask.Common; // 引入游戏任务公共相关的命名空间
using BetterGenshinImpact.Service.Notification; // 引入通知服务相关的命名空间
using BetterGenshinImpact.View.Drawable; // 引入可绘制视图相关的命名空间
using BetterGenshinImpact.ViewModel.Pages; // 引入视图模型页面相关的命名空间
using Microsoft.Extensions.Logging; // 引入日志记录扩展相关的命名空间
using OpenCvSharp; // 引入 OpenCV 的 .NET 封装库
using System; // 引入基础系统功能相关的命名空间
using System.Collections.Generic; // 引入泛型集合相关的命名空间
using System.Threading; // 引入线程相关的命名空间
using System.Threading.Tasks; // 引入异步任务相关的命名空间

namespace BetterGenshinImpact.GameTask.AutoGeniusInvokation.Model; // 定义命名空间

/// <summary>
/// 对局
/// </summary>
public class Duel
{
    private readonly ILogger<Duel> _logger = App.GetLogger<Duel>(); // 获取日志记录器实例

    public Character CurrentCharacter { get; set; } = default!; // 当前角色
    public Character[] Characters { get; set; } = new Character[4]; // 角色数组，最多包含4个角色

    /// <summary>
    /// 行动指令队列
    /// </summary>
    public List<ActionCommand> ActionCommandQueue { get; set; } = []; // 行动指令队列

    /// <summary>
    /// 当前回合数
    /// </summary>
    public int RoundNum { get; set; } = 1; // 当前回合数，初始为1

    /// <summary>
    /// 角色牌位置
    /// </summary>
    public List<Rect> CharacterCardRects { get; set; } = default!; // 角色牌的位置信息

    /// <summary>
    /// 手牌数量
    /// </summary>
    public int CurrentCardCount { get; set; } = 0; // 当前手牌数量

    /// <summary>
    /// 骰子数量
    /// </summary>
    public int CurrentDiceCount { get; set; } = 0; // 当前骰子数量

    public CancellationTokenSource Cts { get; set; } = default!; // 取消操作的令牌源

    private int _keqingECount = 0; // 私有字段，记录特定角色技能使用次数

    public async Task RunAsync(GeniusInvokationTaskParam taskParam)
    {
        await Task.Run(() => { Run(taskParam); }); // 异步运行 Run 方法
    }

    public void Run(GeniusInvokationTaskParam taskParam)
    {
        // 方法体待实现
    }

    private HashSet<ElementalType> PredictionDiceType()
    {
        // 方法体待实现
    }
    {
        // 初始化使用骰子总数为0
        var actionUseDiceSum = 0;
        // 创建一个包含 Omni 元素类型的集合
        var elementSet = new HashSet<ElementalType>
        {
            ElementalType.Omni
        };
        // 遍历行动命令队列
        for (var i = 0; i < ActionCommandQueue.Count; i++)
        {
            // 获取当前行动命令
            var actionCommand = ActionCommandQueue[i];

            // 角色未被打败的情况下才能执行
            if (actionCommand.Character.IsDefeated)
            {
                continue;
            }

            // 通过骰子数量判断是否可以执行

            // 1. 判断切人
            if (i > 0 && actionCommand.Character.Index != ActionCommandQueue[i - 1].Character.Index)
            {
                actionUseDiceSum++;
                if (actionUseDiceSum > CurrentDiceCount)
                {
                    break;
                }
                else
                {
                    // elementSet.Add(actionCommand.GetDiceUseElementType());
                    //executeActionIndex.Add(-actionCommand.Character.Index);
                }
            }

            // 2. 判断使用技能
            actionUseDiceSum += actionCommand.GetAllDiceUseCount();
            if (actionUseDiceSum > CurrentDiceCount)
            {
                break;
            }
            else
            {
                elementSet.Add(actionCommand.GetDiceUseElementType());
                //executeActionIndex.Add(i);
            }
        }

        // 调整元素骰子识别素材顺序
        GeniusInvokationControl.GetInstance().SortActionPhaseDiceMats(elementSet);

        // 返回包含的元素类型集合
        return elementSet;
    }

    // 清除所有角色的状态列表
    public void ClearCharacterStatus()
    {
        foreach (var character in Characters)
        {
            // 清除角色状态列表，如果角色存在且状态列表存在
            character?.StatusList?.Clear();
        }
    }

    /// <summary>
    /// 根据前面执行的命令计算等待时间
    /// 大招等待15秒
    /// 快速切换等待3秒
    /// </summary>
    /// <param name="alreadyExecutedActionCommand"></param>
    /// <returns></returns>
    private int ComputeWaitForMyTurnTime(List<ActionCommand> alreadyExecutedActionCommand)
    {
        foreach (var command in alreadyExecutedActionCommand)
        {
            // 大招的目标索引为1，等待时间为15秒
            if (command.Action == ActionEnum.UseSkill && command.TargetIndex == 1)
            {
                return 15000;
            }

            // 莫娜角色的快速切换等待3秒
            if (command.Character.Name == "莫娜" && command.Action == ActionEnum.SwitchLater)
            {
                return 3000;
            }
        }

        // 默认等待时间为10秒
        return 10000;
    }

    /// <summary>
    /// 获取角色切换顺序
    /// </summary>
    /// <returns></returns>
    public List<int> GetCharacterSwitchOrder()
    {
        List<int> orderList = [];
        // 遍历行动命令队列，获取角色切换顺序
        for (var i = 0; i < ActionCommandQueue.Count; i++)
        {
            // 如果 orderList 不包含当前角色索引，则添加到列表中
            if (!orderList.Contains(ActionCommandQueue[i].Character.Index))
            {
                orderList.Add(ActionCommandQueue[i].Character.Index);
            }
        }

        // 返回角色切换顺序列表
        return orderList;
    }

    ///// <summary>
    ///// 获取角色存活数量
    ///// </summary>
    ///// <returns></returns>
    //public int GetCharacterAliveNum()
    //{
    //    int num = 0;
    // 遍历 Characters 集合中的每个角色
    //    foreach (var character in Characters)
    //    {
    // 检查角色是否不为空且未被击败
    //        if (character != null && !character.IsDefeated)
    //        {
    // 增加计数器，统计存活角色的数量
    //            num++;
    //        }
    //    }

    // 返回存活角色的数量
    //    return num;
    //}

    // 定义一个私有方法，用于记录游戏屏幕分辨率
    private void LogScreenResolution()
    {
        // 获取当前游戏窗口的尺寸
        var gameScreenSize = SystemControl.GetGameScreenRect(TaskContext.Instance().GameHandle);
        // 检查当前游戏窗口的宽度和高度是否为 1920x1080
        if (gameScreenSize.Width != 1920 || gameScreenSize.Height != 1080)
        {
            // 如果分辨率不为 1920x1080，记录警告日志
            _logger.LogWarning("游戏窗口分辨率不是 1920x1080 ！当前分辨率为 {Width}x{Height} , 非 1920x1080 分辨率的游戏可能无法正常使用自动七圣召唤 !", gameScreenSize.Width, gameScreenSize.Height);
        }
    }
# 这行代码是一个闭合的大括号，通常用来标记一个代码块的结束
}
```