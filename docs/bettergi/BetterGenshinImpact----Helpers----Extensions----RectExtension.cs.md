# `.\better-genshin-impact\BetterGenshinImpact\Helpers\Extensions\RectExtension.cs`

```cs
# 引入 OpenCvSharp 库，以便使用其类和功能
﻿using OpenCvSharp;

# 定义命名空间，以组织代码结构
namespace BetterGenshinImpact.Helpers.Extensions;

# 定义静态类，用于扩展现有功能
public static class RectExtension
{
    # 定义扩展方法，判断一个点是否在矩形内
    public static bool Contains(this Rect rect, double x, double y)
    {
        # 判断点 (x, y) 是否在矩形 rect 内
        return (rect.X <= x && rect.Y <= y && rect.X + rect.Width > x && rect.Y + rect.Height > y);
    }
}
```