# `.\better-genshin-impact\BetterGenshinImpact\Helpers\Extensions\PointExtension.cs`

```cs
# 引入 OpenCvSharp 和 System 命名空间
﻿using OpenCvSharp;
using System;

# 定义命名空间 BetterGenshinImpact.Helpers.Extensions
namespace BetterGenshinImpact.Helpers.Extensions;

# 定义一个静态类 PointExtension
public static class PointExtension
{
    # 判断 Point2f 是否为空，即 X 和 Y 是否都为 0
    public static bool IsEmpty(this Point2f point) => point is { X: 0, Y: 0 };

    # 从 Point2f 构建一个 Rect 对象，使用给定的宽度和高度
    public static Rect CenterToRect(this Point2f point, int width, int height) =>
        new((int)Math.Round(point.X - width / 2f, 0), (int)Math.Round(point.Y - height / 2f, 0), width, height);
}
```