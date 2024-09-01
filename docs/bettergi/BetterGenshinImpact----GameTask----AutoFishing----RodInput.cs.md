# `.\better-genshin-impact\BetterGenshinImpact\GameTask\AutoFishing\RodInput.cs`

```cs
# 定义一个命名空间 BetterGenshinImpact.GameTask.AutoFishing
﻿namespace BetterGenshinImpact.GameTask.AutoFishing;

# 定义一个记录类型 RodInput
public record RodInput
{
    # 钓竿在 X 轴的起始坐标
    public double rod_x1;
    # 钓竿在 X 轴的结束坐标
    public double rod_x2;
    # 钓竿在 Y 轴的起始坐标
    public double rod_y1;
    # 钓竿在 Y 轴的结束坐标
    public double rod_y2;
    # 鱼在 X 轴的起始坐标
    public double fish_x1;
    # 鱼在 X 轴的结束坐标
    public double fish_x2;
    # 鱼在 Y 轴的起始坐标
    public double fish_y1;
    # 鱼在 Y 轴的结束坐标
    public double fish_y2;
    # 鱼的标签或标识符
    public int fish_label;
}
```