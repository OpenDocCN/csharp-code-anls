# `.\better-genshin-impact\BetterGenshinImpact\View\Drawable\TextDrawable.cs`

```cs
# 引入必要的命名空间
﻿using System;
using System.Windows;

# 定义命名空间
namespace BetterGenshinImpact.View.Drawable;

# 标记该类可以被序列化
[Serializable]
public class TextDrawable
{
    # 定义一个字符串属性，用于存储文本
    public string Text { get; set; }
    # 定义一个 Point 属性，用于存储坐标
    public Point Point { get; set; }

    # 构造函数，初始化 Text 和 Point 属性
    public TextDrawable(string text, Point point)
    {
        Text = text;
        Point = point;
    }

    # 重写 Equals 方法，判断当前对象是否与另一个对象相等
    public override bool Equals(object? obj)
    {
        # 如果对象为空或类型不匹配，则返回 false
        if (obj == null || GetType() != obj.GetType())
        {
            return false;
        }

        # 将对象转换为 TextDrawable 类型，并比较其 Point 属性
        var other = (TextDrawable)obj;
        return Point.Equals(other.Point);
    }

    # 重写 GetHashCode 方法，返回 Point 属性的哈希码
    public override int GetHashCode()
    {
        return Point.GetHashCode();
    }

    # 定义一个只读属性，用于判断 Point 是否为空（即坐标是否为 (0, 0)）
    public bool IsEmpty => Point is { X: 0, Y: 0 };
}
```