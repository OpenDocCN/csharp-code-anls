# `.\better-genshin-impact\BetterGenshinImpact\GameTask\AutoGeniusInvokation\Assets\AutoGeniusInvokationAssets.cs`

```cs
﻿using BetterGenshinImpact.Core.Recognition; // 引入用于识别的核心库
using BetterGenshinImpact.GameTask.AutoGeniusInvokation.Model; // 引入 AutoGeniusInvokation 模型库
using BetterGenshinImpact.GameTask.Model; // 引入游戏任务模型库
using OpenCvSharp; // 引入 OpenCV 库，用于图像处理
using System.Collections.Generic; // 引入字典集合类
using System.Diagnostics; // 引入诊断类库
using System.Linq; // 引入 LINQ 扩展方法库

namespace BetterGenshinImpact.GameTask.AutoGeniusInvokation.Assets; // 定义命名空间

public class AutoGeniusInvokationAssets : BaseAssets<AutoGeniusInvokationAssets> // 定义继承自 BaseAssets 的类
{
    public RecognitionObject ConfirmButtonRo; // 确认按钮的识别对象
    public RecognitionObject RoundEndButtonRo; // 回合结束按钮的识别对象
    public RecognitionObject ElementalTuningConfirmButtonRo; // 元素调整确认按钮的识别对象
    public RecognitionObject ExitDuelButtonRo; // 退出对战按钮的识别对象

    public RecognitionObject InOpponentActionRo; // 对手行动中的识别对象
    public RecognitionObject EndPhaseRo; // 回合结束阶段的识别对象
    public RecognitionObject ElementalDiceLackWarningRo; // 元素骰子不足警告的识别对象
    public RecognitionObject CharacterTakenOutRo; // 角色被移除的识别对象
    public Mat CharacterDefeatedMat; // 角色失败时的图像矩阵
    public RecognitionObject InCharacterPickRo; // 角色选择中的识别对象

    // 角色区域
    public RecognitionObject CharacterHpUpperRo; // 角色生命值上方区域的识别对象

    public Mat CharacterStatusFreezeMat; // 角色冻结状态的图像矩阵
    public Mat CharacterStatusDizzinessMat; // 角色眩晕状态的图像矩阵
    public Mat CharacterEnergyOnMat; // 角色能量开启的图像矩阵

    public Dictionary<string, Mat> RollPhaseDiceMats; // 回合阶段骰子的图像矩阵字典
    public Dictionary<string, Mat> ActionPhaseDiceMats; // 行动阶段骰子的图像矩阵字典

    private AutoGeniusInvokationAssets() // 私有构造函数
    }
}
```