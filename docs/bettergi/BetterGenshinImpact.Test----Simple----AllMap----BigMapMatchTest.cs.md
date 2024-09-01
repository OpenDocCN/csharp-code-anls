# `.\better-genshin-impact\BetterGenshinImpact.Test\Simple\AllMap\BigMapMatchTest.cs`

```cs
# 引入需要的命名空间
﻿using BetterGenshinImpact.Core.Recognition.OpenCv.FeatureMatch;
using BetterGenshinImpact.GameTask.Common.Element.Assets;
using BetterGenshinImpact.Helpers;
using OpenCvSharp;
using OpenCvSharp.Detail;
using System.Diagnostics;
using System.Windows.Forms;
using BetterGenshinImpact.Core.Recognition.OpenCv;
using BetterGenshinImpact.Helpers.Extensions;

namespace BetterGenshinImpact.Test.Simple.AllMap;

# 定义 BigMapMatchTest 类
public class BigMapMatchTest
{
    # 定义静态测试方法
    public static void Test()
    {
        # 创建一个 SpeedTimer 实例用于计时
        SpeedTimer speedTimer = new();

        # 从指定路径读取灰度图像并创建 Mat 对象，注释掉了实际代码
        // var mainMap100BlockMat = new Mat(@"D:\HuiPrograming\Projects\CSharp\MiHoYo\BetterGenshinImpact\BetterGenshinImpact\Assets\Map\mainMap100Block.png", ImreadModes.Grayscale);

        # 获取实例化的 MainMap2048BlockMat，并将其缩小
        var map2048 = MapAssets.Instance.MainMap2048BlockMat.Value;
        var mainMap100BlockMat = ResizeHelper.Resize(map2048, 1d / (4 * 2));
        
        # 将缩小后的图像写入指定路径
        Cv2.ImWrite(@"E:\HuiTask\更好的原神\地图匹配\有用的素材\mainMap128Block.png", mainMap100BlockMat);

        # 创建 FeatureMatcher 实例用于特征匹配
        var surfMatcher = new FeatureMatcher(mainMap100BlockMat);
        
        # 从指定路径读取灰度图像并创建 Mat 对象
        var queryMat = new Mat(@"E:\HuiTask\更好的原神\地图匹配\比较\Clip_20240321_000329.png", ImreadModes.Grayscale);

        # 记录初始化特征所需的时间
        speedTimer.Record("初始化特征");

        # 将查询图像缩小
        queryMat = ResizeHelper.Resize(queryMat, 1d / 4);
        
        # 显示查询图像
        Cv2.ImShow("queryMat", queryMat);

        # 执行特征匹配
        var p = surfMatcher.Match(queryMat);
        
        # 记录特征匹配过程所需的时间
        speedTimer.Record("匹配1");
        
        # 检查匹配结果是否为空
        if (p.IsEmpty())
        {
            # 如果匹配为空，打印调试信息
            Debug.WriteLine($"Matched rect 1: {p}");
            # 注释掉了绘制矩形框的代码
            // var rect = Cv2.BoundingRect(pArray);
            // Cv2.Rectangle(mainMap100BlockMat, rect, Scalar.Red, 2);
            // Cv2.ImShow(@"b1", mainMap100BlockMat);
        }
        else
        {
            # 如果匹配不为空，打印调试信息
            Debug.WriteLine("No match 1");
        }
        
        # 打印速度计时器的调试信息
        speedTimer.DebugPrint();
    }
}
```