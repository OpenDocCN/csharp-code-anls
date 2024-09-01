# `.\better-genshin-impact\BetterGenshinImpact\Core\Recognition\OCR\PaddleOcrResultExtension.cs`

```cs
# 使用 BetterGenshinImpact 和其他库的引用
﻿using BetterGenshinImpact.GameTask.AutoSkip.Model;
using BetterGenshinImpact.View.Drawable;
using OpenCvSharp;
using Sdcb.PaddleOCR;
using System;
using System.Collections.Generic;
using System.Drawing;
using System.Linq;

# 定义命名空间 BetterGenshinImpact.Core.Recognition.OCR
namespace BetterGenshinImpact.Core.Recognition.OCR;

# 定义静态类 PaddleOcrResultExtension
public static class PaddleOcrResultExtension
{
    # 扩展方法，检查 PaddleOcrResult 中是否包含指定文本
    public static bool RegionHasText(this PaddleOcrResult result, ReadOnlySpan<char> text)
    {
        # 遍历 OCR 结果中的每个区域
        foreach (ref readonly PaddleOcrResultRegion item in result.Regions.AsSpan())
        {
            # 检查区域文本是否包含指定文本
            if (item.Text.AsSpan().Contains(text, StringComparison.InvariantCulture))
            {
                return true;
            }
        }

        # 如果没有找到指定文本，则返回 false
        return false;
    }

    # 扩展方法，找到包含指定文本的区域
    public static PaddleOcrResultRegion FindRegionByText(this PaddleOcrResult result, ReadOnlySpan<char> text)
    {
        # 遍历 OCR 结果中的每个区域
        foreach (ref readonly PaddleOcrResultRegion item in result.Regions.AsSpan())
        {
            # 检查区域文本是否包含指定文本
            if (item.Text.AsSpan().Contains(text, StringComparison.InvariantCulture))
            {
                return item;
            }
        }

        # 如果没有找到指定文本，则返回默认值
        return default;
    }

    # 扩展方法，找到包含指定文本的区域的矩形
    public static Rect FindRectByText(this PaddleOcrResult result, string text)
    {
        # 遍历 OCR 结果中的每个区域
        foreach (ref PaddleOcrResultRegion item in result.Regions.AsSpan())
        {
            # 检查区域文本是否包含指定文本
            if (item.Text.Contains(text))
            {
                return item.Rect.BoundingRect();
            }
        }

        # 如果没有找到指定文本，则返回默认值
        return default;
    }

    # 扩展方法，将每个区域的矩形转换为 RectDrawable 列表
    public static List<RectDrawable> ToRectDrawableList(this PaddleOcrResult result, Pen? pen = null)
    {
        # 将每个区域的矩形转换为 RectDrawable 并生成列表
        return result.Regions.Select(item => item.Rect.BoundingRect().ToRectDrawable(pen)).ToList();
    }

    # 扩展方法，将每个区域的矩形转换为带有偏移的 RectDrawable 列表
    public static List<RectDrawable> ToRectDrawableListOffset(this PaddleOcrResult result, int offsetX, int offsetY, Pen? pen = null)
    {
        # 将每个区域的矩形转换为带有偏移的 RectDrawable 并生成列表
        return result.Regions.Select(item => item.Rect.BoundingRect().ToRectDrawable(offsetX, offsetY, pen)).ToList();
    }

    # 扩展方法，将 PaddleOcrResultRegion 转换为 PaddleOcrResultRect
    public static PaddleOcrResultRect ToOcrResultRect(this PaddleOcrResultRegion region)
    {
        # 使用区域的矩形、文本和得分创建 PaddleOcrResultRect 对象
        return new PaddleOcrResultRect(region.Rect.BoundingRect(), region.Text, region.Score);
    }
}
```