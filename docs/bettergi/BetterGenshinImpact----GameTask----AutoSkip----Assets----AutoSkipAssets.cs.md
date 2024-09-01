# `.\better-genshin-impact\BetterGenshinImpact\GameTask\AutoSkip\Assets\AutoSkipAssets.cs`

```cs
# 引入 BetterGenshinImpact 核心识别模块
﻿using BetterGenshinImpact.Core.Recognition;
# 引入 BetterGenshinImpact 游戏任务模型
using BetterGenshinImpact.GameTask.Model;
# 引入 OpenCVSharp 库
using OpenCvSharp;

# 定义 BetterGenshinImpact.GameTask.AutoSkip.Assets 命名空间
namespace BetterGenshinImpact.GameTask.AutoSkip.Assets;

# 定义 AutoSkipAssets 类，继承自 BaseAssets
public class AutoSkipAssets : BaseAssets<AutoSkipAssets>
{
    # 定义 RecognitionObject 类型的字段，用于识别停止自动按钮
    public RecognitionObject StopAutoButtonRo;

    # 定义 RecognitionObject 类型的字段，用于识别聊天审核按钮 (已注释)
    // public RecognitionObject ChatReviewButtonRo;
    # 定义 RecognitionObject 类型的字段，用于识别禁用 UI 按钮
    public RecognitionObject DisabledUiButtonRo;

    # 定义 RecognitionObject 类型的字段，用于识别播放文本
    public RecognitionObject PlayingTextRo;

    # 定义 Rect 类型的字段，用于指定选项区域的矩形
    public Rect OptionRoi;
    # 定义 RecognitionObject 类型的字段，用于识别选项图标
    public RecognitionObject OptionIconRo;
    # 定义 RecognitionObject 类型的字段，用于识别每日奖励图标
    public RecognitionObject DailyRewardIconRo;
    # 定义 RecognitionObject 类型的字段，用于识别探索图标
    public RecognitionObject ExploreIconRo;
    # 定义 RecognitionObject 类型的字段，用于识别感叹号图标
    public RecognitionObject ExclamationIconRo;

    # 定义 RecognitionObject 类型的字段，用于识别页面关闭图标
    public RecognitionObject PageCloseRo;

    # 定义 RecognitionObject 类型的字段，用于识别收集按钮
    public RecognitionObject CollectRo;
    # 定义 RecognitionObject 类型的字段，用于识别重新按钮
    public RecognitionObject ReRo;

    # 定义 RecognitionObject 类型的字段，用于识别原石图标
    public RecognitionObject PrimogemRo;

    # 定义 RecognitionObject 类型的字段，用于识别提交感叹号图标
    public RecognitionObject SubmitExclamationIconRo;
    # 定义 RecognitionObject 类型的字段，用于识别提交物品按钮
    public RecognitionObject SubmitGoodsRo;
    # 定义 Mat 类型的字段，用于存储提交物品的图像矩阵
    public Mat SubmitGoodsMat;

    # 定义 Mat 类型的字段，用于存储选择挂件的图像矩阵 (已注释)
    // public Mat HangoutSelectedMat;
    # 定义 Mat 类型的字段，用于存储未选择挂件的图像矩阵 (已注释)
    // public Mat HangoutUnselectedMat;
    # 定义 RecognitionObject 类型的字段，用于识别选择挂件
    public RecognitionObject HangoutSelectedRo;

    # 定义 RecognitionObject 类型的字段，用于识别未选择挂件
    public RecognitionObject HangoutUnselectedRo;
    # 定义 RecognitionObject 类型的字段，用于识别跳过挂件
    public RecognitionObject HangoutSkipRo;

    # 定义私有构造函数，防止外部实例化
    private AutoSkipAssets()
    }
}
```