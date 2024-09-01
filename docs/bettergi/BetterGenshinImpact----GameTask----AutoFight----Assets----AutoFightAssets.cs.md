# `.\better-genshin-impact\BetterGenshinImpact\GameTask\AutoFight\Assets\AutoFightAssets.cs`

```cs
# 引入 BetterGenshinImpact.Core.Recognition 命名空间
using BetterGenshinImpact.Core.Recognition;
# 引入 BetterGenshinImpact.GameTask.Model 命名空间
using BetterGenshinImpact.GameTask.Model;
# 引入 OpenCvSharp 命名空间
using OpenCvSharp;
# 引入 System.Collections.Generic 命名空间，用于使用泛型集合
using System.Collections.Generic;

# 定义命名空间 BetterGenshinImpact.GameTask.AutoFight.Assets
namespace BetterGenshinImpact.GameTask.AutoFight.Assets
{
    # 定义 AutoFightAssets 类，继承自 BaseAssets 类
    public class AutoFightAssets : BaseAssets<AutoFightAssets>
    {
        # 定义一个矩形区域，不带索引
        public Rect TeamRectNoIndex;
        # 定义一个矩形区域，带索引
        public Rect TeamRect;
        # 定义一个包含多个矩形的列表，表示头像侧边图标的位置
        public List<Rect> AvatarSideIconRectList;
        # 定义一个包含多个矩形的列表，表示头像索引的位置
        public List<Rect> AvatarIndexRectList;
        # 定义一个矩形区域，表示技能 E 的位置
        public Rect ERect;
        # 定义一个矩形区域，表示技能 Q 的位置
        public Rect QRect;
        # 定义一个矩形区域，表示挑战达成提示的位置
        public Rect EndTipsUpperRect; // 挑战达成提示
        # 定义一个矩形区域，表示挑战完成提示的位置
        public Rect EndTipsRect;
        # 定义一个识别对象，用于识别 Wanderer 图标
        public RecognitionObject WandererIconRa;
        # 定义一个识别对象，用于识别未激活的 Wanderer 图标
        public RecognitionObject WandererIconNoActiveRa;
        # 定义一个识别对象，用于确认按钮的识别
        public RecognitionObject ConfirmRa;
        # 定义一个识别对象，用于退出按钮的识别
        public RecognitionObject ExitRa;
        # 定义一个识别对象，用于点击任何关闭提示的识别
        public RecognitionObject ClickAnyCloseTipRa;
        # 定义一个识别对象，用于使用浓缩树脂的识别
        public RecognitionObject UseCondensedResinRa;

        # 定义一个识别对象，用于树脂状态的识别
        public RecognitionObject CondensedResinCountRa;

        # 定义一个识别对象，用于易碎树脂计数的识别
        public RecognitionObject FragileResinCountRa;

        # 定义一个字典，用于存储头像与服装的映射关系
        public Dictionary<string, string> AvatarCostumeMap;

        # 私有构造函数，防止外部实例化
        private AutoFightAssets()
        }
}
```