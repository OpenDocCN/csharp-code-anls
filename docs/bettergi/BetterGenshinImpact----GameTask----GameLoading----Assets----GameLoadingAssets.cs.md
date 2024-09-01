# `.\better-genshin-impact\BetterGenshinImpact\GameTask\GameLoading\Assets\GameLoadingAssets.cs`

```cs
# 导入 BetterGenshinImpact.Core.Recognition 命名空间
using BetterGenshinImpact.Core.Recognition;
# 导入 BetterGenshinImpact.GameTask.Model 命名空间
using BetterGenshinImpact.GameTask.Model;
# 导入 OpenCvSharp 命名空间
using OpenCvSharp;

# 定义命名空间 BetterGenshinImpact.GameTask.GameLoading.Assets
namespace BetterGenshinImpact.GameTask.GameLoading.Assets
{
    # 定义 GameLoadingAssets 类，继承自 BaseAssets<GameLoadingAssets>
    public class GameLoadingAssets : BaseAssets<GameLoadingAssets>
    {
        # 声明 EnterGameRo 成员变量，类型为 RecognitionObject
        public RecognitionObject EnterGameRo;
        # 声明 WelkinMoonRo 成员变量，类型为 RecognitionObject
        public RecognitionObject WelkinMoonRo;

        # 定义 GameLoadingAssets 的构造函数
        private GameLoadingAssets()
        {
            # 初始化 EnterGameRo 对象
            EnterGameRo = new RecognitionObject
            {
                # 设置识别对象的名称为 "EnterGame"
                Name = "EnterGame",
                # 设置识别类型为模板匹配
                RecognitionType = RecognitionTypes.TemplateMatch,
                # 加载模板图像并设置为识别对象的模板图像
                TemplateImageMat = GameTaskManager.LoadAssetImage("AutoWood", "exit_welcome.png"),
                # 设置感兴趣区域为图像下半部分
                RegionOfInterest = new Rect(0, CaptureRect.Height / 2, CaptureRect.Width, CaptureRect.Height - CaptureRect.Height / 2),
                # 不在窗口上绘制识别区域
                DrawOnWindow = false
            # 初始化模板
            }.InitTemplate();

            # 初始化 WelkinMoonRo 对象
            WelkinMoonRo = new RecognitionObject
            {
                # 设置识别对象的名称为 "WelkinMoon"
                Name = "WelkinMoon",
                # 设置识别类型为模板匹配
                RecognitionType = RecognitionTypes.TemplateMatch,
                # 加载模板图像并设置为识别对象的模板图像
                TemplateImageMat = GameTaskManager.LoadAssetImage("GameLoading", "welkin_moon_logo.png"),
                # 设置感兴趣区域为图像上半部分
                RegionOfInterest = new Rect(0, CaptureRect.Height / 2, CaptureRect.Width, CaptureRect.Height / 2),
                # 不在窗口上绘制识别区域
                DrawOnWindow = false
            # 初始化模板
            }.InitTemplate();
        }
    }
}
```