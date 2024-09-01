# `.\better-genshin-impact\BetterGenshinImpact\GameTask\Model\Area\Converter\ConvertRes.cs`

```cs
# 引入 OpenCvSharp 库和 System 命名空间
﻿using OpenCvSharp;
using System;

# 定义命名空间
namespace BetterGenshinImpact.GameTask.Model.Area.Converter;

# 定义泛型类 ConvertRes，T 是一个类型参数，要求是 Region 类型或其子类
public class ConvertRes<T>(int x, int y, int width, int height, T node) where T : Region
{
    # 定义 X 属性并初始化为构造函数传入的 x 值
    public int X { get; set; } = x;
    # 定义 Y 属性并初始化为构造函数传入的 y 值
    public int Y { get; set; } = y;
    # 定义 Width 属性并初始化为构造函数传入的 width 值
    public int Width { get; set; } = width;
    # 定义 Height 属性并初始化为构造函数传入的 height 值
    public int Height { get; set; } = height;

    # 定义 TargetRegion 属性并初始化为构造函数传入的 node
    public T TargetRegion { get; set; } = node;

    # 定义将当前对象转换为 Rect 类型的方法
    public Rect ToRect()
    {
        # 返回一个 Rect 对象，使用当前对象的 X, Y, Width 和 Height 属性
        return new Rect(X, Y, Width, Height);
    }

    # 定义一个静态方法，用于将坐标和尺寸转换为目标区域
    public static ConvertRes<T> ConvertPositionToTargetRegion(int x, int y, int w, int h, Region startNode)
    {
        # 初始化 node 为 startNode
        var node = startNode;
        # 遍历链表，直到找到类型为 T 的节点
        while (node != null)
        {
            # 如果当前节点是类型 T 的实例，退出循环
            if (node is T)
            {
                break;
            }

            # 如果当前节点有 PrevConverter 属性，使用它进行坐标转换
            if (node.PrevConverter != null)
            {
                (x, y, w, h) = node.PrevConverter.ToPrev(x, y, w, h);
            }
            # 如果 PrevConverter 为空，抛出异常
            else
            {
                throw new Exception("PrevConverter is null");
            }

            # 更新 node 为前一个节点
            node = node.Prev;
        }

        # 如果 node 是类型 T 的实例，返回新的 ConvertRes 对象
        if (node is T targetRegion)
        {
            return new ConvertRes<T>(x, y, w, h, targetRegion);
        }
        # 否则，抛出异常
        else
        {
            throw new Exception("Target Region not found");
        }
    }
}
```