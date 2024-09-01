# `.\better-genshin-impact\BetterGenshinImpact\GameTask\Common\Map\CharacterOrientation.cs`

```cs
# 引入 OpenCvSharp 库和系统库
﻿using OpenCvSharp;
using System;

namespace BetterGenshinImpact.GameTask.Common.Map;

/// <summary>
/// 以左上角为原点的图像坐标系下的角度计算
/// </summary>
public class CharacterOrientation
{
    # 计算图像的角度，方法尚未实现
    public static int Compute(Mat mat)
    }

    # 计算两个点的中点
    static Point Midpoint(Point p1, Point p2)
    {
        # 计算中点的 X 坐标
        var midX = (p1.X + p2.X) / 2;
        # 计算中点的 Y 坐标
        var midY = (p1.Y + p2.Y) / 2;
        # 返回新的中点
        return new Point(midX, midY);
    }

    # 将图像路径转换为角度
    public static int GameAngle2(string path)
    {
        # 读取指定路径的图像
        var mat = Cv2.ImRead(path);
        # 计算图像的角度
        return Compute(mat);
    }
}
```