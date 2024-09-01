# `.\better-genshin-impact\BetterGenshinImpact\GameTask\Model\Area\Converter\TranslationConverter.cs`

```cs
# 命名空间定义
﻿namespace BetterGenshinImpact.GameTask.Model.Area.Converter;

# 提供平移变换的转换器类
/// <summary>
/// 平移变换
/// </summary>
# 定义一个平移变换的转换器类，实现了 INodeConverter 接口
public class TranslationConverter(int offsetX, int offsetY) : INodeConverter
{
    # 将给定的坐标和尺寸应用平移变换
    public (int x, int y, int w, int h) ToPrev(int x, int y, int w, int h)
    {
        # 返回应用平移变换后的坐标和尺寸
        return (x + offsetX, y + offsetY, w, h);
    }
}
```