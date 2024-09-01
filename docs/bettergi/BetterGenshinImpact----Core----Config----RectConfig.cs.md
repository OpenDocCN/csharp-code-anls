# `.\better-genshin-impact\BetterGenshinImpact\Core\Config\RectConfig.cs`

```cs
# 引入 OpenCvSharp 库
﻿using OpenCvSharp;
# 引入系统库
using System;

# 定义命名空间
namespace BetterGenshinImpact.Core.Config;

# 标记该类为过时，建议使用原始的 OpenCvSharp.Rect
[Obsolete("Replace with original OpenCvSharp.Rect")]
# 定义一个配置类 RectConfig
public class RectConfig
{
    # 默认构造函数
    public RectConfig()
    {
    }

    # 带参数的构造函数，用于初始化矩形的各个属性
    public RectConfig(int x, int y, int width, int height)
    {
        X = x;
        Y = y;
        Width = width;
        Height = height;
    }

    # 矩形的 X 坐标
    public int X { get; set; }
    # 矩形的 Y 坐标
    public int Y { get; set; }
    # 矩形的宽度
    public int Width { get; set; }
    # 矩形的高度
    public int Height { get; set; }

    # 将当前 RectConfig 实例转换为 OpenCvSharp.Rect 对象
    public Rect ToRect()
    {
        return new Rect(X, Y, Width, Height);
    }
}
```