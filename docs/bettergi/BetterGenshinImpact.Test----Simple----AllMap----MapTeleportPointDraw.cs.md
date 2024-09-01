# `.\better-genshin-impact\BetterGenshinImpact.Test\Simple\AllMap\MapTeleportPointDraw.cs`

```cs
﻿using BetterGenshinImpact.Service;  // 引入 BetterGenshinImpact.Service 命名空间
using OpenCvSharp;  // 引入 OpenCvSharp 命名空间，用于图像处理
using System.Diagnostics;  // 引入 System.Diagnostics 命名空间，用于调试
using System.IO;  // 引入 System.IO 命名空间，用于文件操作
using System.Text.Json;  // 引入 System.Text.Json 命名空间，用于 JSON 序列化和反序列化

namespace BetterGenshinImpact.Test.Simple.AllMap;  // 定义命名空间 BetterGenshinImpact.Test.Simple.AllMap

public class MapTeleportPointDraw  // 定义 MapTeleportPointDraw 类
{
    public static void Draw()  // 静态方法 Draw，用于绘制传送点
    {
        // 从指定路径加载传送点数据
        var pList = LoadTeleportPoint(@"E:\HuiTask\更好的原神\地图匹配\地图点位\Json_Integration\锚点&神像\");
        // 加载指定路径的地图图像
        var map = new Mat(@"E:\HuiTask\更好的原神\地图匹配\genshin_map_带标记.png");
        // 在地图上绘制传送点
        DrawTeleportPoint(map, pList.ToArray());
        // 将绘制了传送点的地图图像保存到指定路径
        Cv2.ImWrite(@"E:\HuiTask\更好的原神\地图匹配\genshin_map_带标记_传送点.png", map);
    }

    public static void DrawTeleportPoint(Mat map, Point[] points)  // 静态方法 DrawTeleportPoint，用于在地图上绘制传送点
    {
        foreach (var point in points)  // 遍历所有传送点
        {
            // 在地图上绘制红色圆圈表示传送点，半径为10，线宽为2
            Cv2.Circle(map, new Point(point.X, point.Y), 10, Scalar.Red, 2);
        }
    }

    public static List<Point> LoadTeleportPoint(string folderPath)  // 静态方法 LoadTeleportPoint，从指定文件夹加载传送点数据
    {
        // 获取指定文件夹下所有文件的路径
        string[] files = Directory.GetFiles(folderPath, "*.*", SearchOption.AllDirectories);
        List<Point> points = new();  // 创建一个列表用于存储传送点
        foreach (var file in files)  // 遍历所有文件
        {
            // 反序列化文件内容为 GamePoint 对象
            var gamePoint = JsonSerializer.Deserialize<GamePoint>(File.ReadAllText(file), ConfigService.JsonOptions);
            if (gamePoint == null)  // 如果反序列化结果为 null
            {
                // 输出调试信息，提示 JSON 数据为空
                Debug.WriteLine($"{file} json is null");
                continue;  // 继续处理下一个文件
            }

            // 将传送点的位置转换为地图坐标并添加到列表中
            points.Add(Transform(gamePoint.Position));
        }

        // 返回传送点列表
        return points;
    }

    /// <summary>
    /// 坐标系转换
    /// </summary>
    /// <param name="position">[a,b,c]</param>
    /// <returns></returns>
    public static Point Transform(decimal[] position)  // 静态方法 Transform，将坐标系转换为地图坐标
    {
        // 四舍五入转换为整数
        var a = (int)Math.Round(position[0]); // 上
        var c = (int)Math.Round(position[2]); // 左

        // 转换1024区块坐标，大地图坐标系正轴是往左上方向的
        // 这里写最左上角的区块坐标(m,n)/(上,左),截止4.5版本，最左上角的区块坐标是(5,7)

        int m = 5, n = 7;  // 定义最左上角的区块坐标
        // 计算并返回地图坐标
        return new Point((n + 1) * 1024 - c, (m + 1) * 1024 - a);
    }

    class GamePoint  // 定义 GamePoint 类，用于表示传送点
    {
        public string Description { get; set; } = string.Empty;  // 描述
        public string Name { get; set; } = string.Empty;  // 名称
        public decimal[] Position { get; set; } = new decimal[3];  // 位置坐标
    }
}
```