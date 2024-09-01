# `.\better-genshin-impact\BetterGenshinImpact\GameTask\Common\Element\Assets\ElementAssets.cs`

```cs
# 引入 BetterGenshinImpact.Core.Recognition 命名空间，用于识别相关功能
using BetterGenshinImpact.Core.Recognition;
# 引入 BetterGenshinImpact.GameTask.Model 命名空间，包含与游戏任务相关的模型
using BetterGenshinImpact.GameTask.Model;
# 引入 OpenCvSharp 命名空间，用于图像处理和计算机视觉功能
using OpenCvSharp;

# 定义 ElementAssets 类，属于 BetterGenshinImpact.GameTask.Common.Element.Assets 命名空间
namespace BetterGenshinImpact.GameTask.Common.Element.Assets
{
    # 定义 ElementAssets 类，继承自 BaseAssets<ElementAssets>
    public class ElementAssets : BaseAssets<ElementAssets>
    {
        # 声明 RecognitionObject 类型的字段，用于识别各种按钮
        public RecognitionObject BtnWhiteConfirm;
        public RecognitionObject BtnWhiteCancel;
        public RecognitionObject BtnBlackConfirm;
        public RecognitionObject BtnBlackCancel;
        public RecognitionObject BtnOnlineYes;
        public RecognitionObject BtnOnlineNo;

        # 声明 RecognitionObject 类型的字段，用于识别其他界面元素
        public RecognitionObject PaimonMenuRo;
        public RecognitionObject BlueTrackPoint;

        public RecognitionObject UiLeftTopCookIcon;

        public RecognitionObject SpaceKey;
        public RecognitionObject XKey;

        public RecognitionObject FriendChat;

        # 定义私有构造函数，防止类的外部实例化
        private ElementAssets()
        {
        }
    }
}
```