# `.\better-genshin-impact\BetterGenshinImpact.Test\Simple\Track\MapPathTest.cs`

```cs
﻿using BetterGenshinImpact.Core.Config;  // 引入 BetterGenshinImpact.Core.Config 命名空间
using BetterGenshinImpact.Core.Recognition.OpenCv;  // 引入 BetterGenshinImpact.Core.Recognition.OpenCv 命名空间
using BetterGenshinImpact.GameTask.AutoTrackPath.Model;  // 引入 BetterGenshinImpact.GameTask.AutoTrackPath.Model 命名空间
using BetterGenshinImpact.GameTask.Common.Element.Assets;  // 引入 BetterGenshinImpact.GameTask.Common.Element.Assets 命名空间
using BetterGenshinImpact.Service;  // 引入 BetterGenshinImpact.Service 命名空间
using OpenCvSharp;  // 引入 OpenCvSharp 命名空间
using System.IO;  // 引入 System.IO 命名空间
using System.Text.Json;  // 引入 System.Text.Json 命名空间

namespace BetterGenshinImpact.Test.Simple.Track;  // 定义命名空间 BetterGenshinImpact.Test.Simple.Track

internal class MapPathTest  // 定义内部类 MapPathTest
{
    public static void Test()  // 定义公共静态方法 Test
    {
        // var wayJson = File.ReadAllText(Global.Absolute(@"log\way\yl3.json"));  // 读取指定路径 JSON 文件的内容作为字符串
        // var way = JsonSerializer.Deserialize<GiPath>(wayJson, ConfigService.JsonOptions) ?? throw new Exception("way json deserialize failed");  // 将 JSON 字符串反序列化为 GiPath 对象，如果失败则抛出异常
        //
        // var points = way.WayPointList.Select(giPathPoint => giPathPoint.MatchRect.GetCenterPoint()).ToList();  // 从 GiPath 对象中提取所有点的中心点，生成点的列表
        //
        // var pointsRect = Cv2.BoundingRect(points);  // 计算包含所有点的最小矩形
        // var allMap = new Mat(@"E:\HuiTask\更好的原神\地图匹配\有用的素材\mainMap2048Block.png");  // 从指定路径加载地图图像
        //
        // // 按顺序连线，每个点都画圈
        // for (var i = 0; i < points.Count - 1; i++)  // 遍历点列表中的每对相邻点
        // {
        //     Cv2.Line(allMap, points[i], points[i + 1], Scalar.Red, 1);  // 在地图上绘制两点之间的红色线段
        //     Cv2.Circle(allMap, points[i], 3, Scalar.Red, -1);  // 在地图上绘制一个红色圆圈表示当前点
        // }
        //
        // var map = allMap[new Rect(pointsRect.X - 100, pointsRect.Y - 100, pointsRect.Width + 200, pointsRect.Height + 200)];  // 从地图图像中裁剪出包含所有点的区域，并扩大边界
        // Cv2.ImShow("map", map);  // 显示裁剪后的地图图像
    }
}
```