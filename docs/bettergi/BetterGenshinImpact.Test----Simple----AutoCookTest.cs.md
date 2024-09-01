# `.\better-genshin-impact\BetterGenshinImpact.Test\Simple\AutoCookTest.cs`

```cs
# 引入 OpenCvSharp 命名空间
﻿using OpenCvSharp;

# 定义命名空间
namespace BetterGenshinImpact.Test.Simple;

# 定义内部类 AutoCookTest
internal class AutoCookTest
{
    # 定义静态方法 Test
    public static void Test()
    {
        # 创建 Mat 对象，并从指定路径加载图像
        var img = new Mat(@"E:\HuiTask\更好的原神\自动烹饪\2.png");
        # 将图像从 BGR 色彩空间转换为 RGB 色彩空间
        Cv2.CvtColor(img, img, ColorConversionCodes.BGR2RGB);
        # 创建一个新的 Mat 对象，用于存储处理后的图像
        var img2 = new Mat();
        # 使用颜色范围检测函数，将指定范围的颜色转换为白色，其它颜色为黑色（注释掉的代码为一个不相同的颜色范围）
        Cv2.InRange(img, new Scalar(255, 255, 192), new Scalar(255, 255, 192), img2);

        # 显示处理后的图像，窗口标题为 "img"
        Cv2.ImShow("img", img2);
    }
}
```