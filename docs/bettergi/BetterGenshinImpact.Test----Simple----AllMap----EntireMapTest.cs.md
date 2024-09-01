# `.\better-genshin-impact\BetterGenshinImpact.Test\Simple\AllMap\EntireMapTest.cs`

```cs
# 引入所需的命名空间
﻿using BetterGenshinImpact.Core.Recognition.OpenCv.FeatureMatch;
using BetterGenshinImpact.GameTask.Common.Element.Assets;
using BetterGenshinImpact.Helpers;
using BetterGenshinImpact.Helpers.Extensions;
using OpenCvSharp;
using OpenCvSharp.Detail;
using System.Diagnostics;
using System.Windows.Forms;

# 定义命名空间
namespace BetterGenshinImpact.Test.Simple.AllMap;

# 定义类
public class EntireMapTest
{
    # 定义静态方法 Test
    public static void Test()
    {
        # 创建 SpeedTimer 实例
        SpeedTimer speedTimer = new();
        # 读取图像文件并创建灰度 Mat 对象
        var mainMap1024BlockMat = new Mat(@"E:\HuiTask\更好的原神\地图匹配\有用的素材\mainMap1024Block.png", ImreadModes.Grayscale);
        # 使用主地图 Mat 创建特征匹配器
        var surfMatcher = new FeatureMatcher(mainMap1024BlockMat);
        # 读取查询图像文件并创建灰度 Mat 对象
        var queryMat = new Mat(@"E:\HuiTask\更好的原神\地图匹配\比较\小地图\Clip_20240323_183119.png", ImreadModes.Grayscale);

        # 记录初始化特征的时间
        speedTimer.Record("初始化特征");

        # 执行特征匹配
        var p = surfMatcher.Match(queryMat);
        # 记录第一次匹配的时间
        speedTimer.Record("匹配1");
        # 检查第一次匹配是否成功
        if (!p.IsEmpty())
        {
            # 输出第一次匹配的结果
            Debug.WriteLine($"Matched rect 1: {p}");
            # 在主地图上标记匹配位置
            Cv2.Circle(mainMap1024BlockMat, p.ToPoint(), 10, Scalar.Red);
            # 注释掉保存图像的代码

            # 执行第二次匹配
            var p2 = surfMatcher.Match(queryMat, p.X, p.Y);
            # 记录第二次匹配的时间
            speedTimer.Record("匹配2");
            # 检查第二次匹配是否成功
            if (!p2.IsEmpty())
            {
                # 输出第二次匹配的结果
                Debug.WriteLine($"Matched rect 2: {p2}");
                # 注释掉绘制矩形和保存图像的代码
            }
            else
            {
                # 输出未匹配到结果的消息
                Debug.WriteLine("No match 2");
            }
        }
        else
        {
            # 输出第一次匹配失败的消息
            Debug.WriteLine("No match 1");
        }
        # 输出速度计时器的调试信息
        speedTimer.DebugPrint();
    }

    # 定义静态方法 Storage
    public static void Storage()
    {
        # 创建特征匹配器实例，并显示完成消息框
        var featureMatcher = new FeatureMatcher(MapAssets.Instance.MainMap2048BlockMat.Value, new FeatureStorage("mainMap2048Block"));
        MessageBox.Show("特征点生成完成");
    }
}
```