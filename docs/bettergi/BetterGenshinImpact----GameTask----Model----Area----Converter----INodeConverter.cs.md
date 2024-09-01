# `.\better-genshin-impact\BetterGenshinImpact\GameTask\Model\Area\Converter\INodeConverter.cs`

```cs
# 定义一个接口，位于 BetterGenshinImpact.GameTask.Model.Area.Converter 命名空间
﻿namespace BetterGenshinImpact.GameTask.Model.Area.Converter;

# 声明接口 INodeConverter
public interface INodeConverter
{
    # 定义一个方法，将当前坐标 (x, y) 和尺寸 (w, h) 转换为上一个状态的坐标和尺寸
    public (int x, int y, int w, int h) ToPrev(int x, int y, int w, int h);

    # 注释掉的代码：定义一个方法，将当前坐标 (x, y) 和尺寸 (w, h) 转换为下一个状态的坐标和尺寸
    // public (int x, int y, int w, int h) ToNext(int x, int y, int w, int h);
}
```