# `.\better-genshin-impact\BetterGenshinImpact\GameTask\Model\Area\Converter\ScaleConverter.cs`

```cs
# 定义命名空间 BetterGenshinImpact.GameTask.Model.Area.Converter
﻿namespace BetterGenshinImpact.GameTask.Model.Area.Converter;

/// <summary>
/// 比例变换类，用于对坐标和尺寸进行比例缩放
/// </summary>
/// <param name="scale">需要放大多少倍</param>
public class ScaleConverter(double scale) : INodeConverter
{
    # 实现 INodeConverter 接口中的 ToPrev 方法，用于将原始坐标和尺寸按照比例缩放
    public (int x, int y, int w, int h) ToPrev(int x, int y, int w, int h)
    {
        # 根据 scale 参数缩放 x、y、w、h 的值，并转换为整数，返回缩放后的坐标和尺寸
        return ((int)(x * scale), (int)(y * scale), (int)(w * scale), (int)(h * scale));
        # 注释掉的代码行：可能是为了保留原始 x 和 y 值不变，仅缩放 w 和 h
        // return (x, y, (int)(w * scale), (int)(h * scale));
    }
}
```