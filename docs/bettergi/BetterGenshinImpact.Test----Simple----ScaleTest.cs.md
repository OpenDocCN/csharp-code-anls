# `.\better-genshin-impact\BetterGenshinImpact.Test\Simple\ScaleTest.cs`

```cs
﻿using OpenCvSharp;

namespace BetterGenshinImpact.Test.Simple;

public class ScaleTest
{
    public static void ZoomOutTest()
    {
        // 从指定路径读取灰度图像，并创建 Mat 对象
        var mainMap2048BlockMat = new Mat(@"E:\HuiTask\更好的原神\地图匹配\有用的素材\5.0\mainMap2048Block.png", ImreadModes.Grayscale);
        // 定义缩小后的目标文件路径
        var targetFilePath = @"E:\HuiTask\更好的原神\地图匹配\有用的素材\5.0\mainMap256Block.png";
        // 使用 OpenCV 的 Resize 函数将图像缩小 8 倍（2048/256 = 8）
        var mainMap256BlockMat = mainMap2048BlockMat.Resize(new Size(mainMap2048BlockMat.Width / 8, mainMap2048BlockMat.Height / 8));
        // 将缩小后的图像保存到目标路径
        mainMap256BlockMat.SaveImage(targetFilePath);
    }
}
```