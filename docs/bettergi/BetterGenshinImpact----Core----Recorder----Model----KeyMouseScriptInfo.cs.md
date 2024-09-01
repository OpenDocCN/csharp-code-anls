# `.\better-genshin-impact\BetterGenshinImpact\Core\Recorder\Model\KeyMouseScriptInfo.cs`

```cs
# 引入 System 命名空间
﻿using System;

# 定义命名空间 BetterGenshinImpact.Core.Recorder.Model
namespace BetterGenshinImpact.Core.Recorder.Model;

# 标记类为可序列化
[Serializable]
# 定义 KeyMouseScriptInfo 类
public class KeyMouseScriptInfo
{
    # 定义 Name 属性，初始化为空字符串
    public string Name { get; set; } = string.Empty;

    # 定义 Description 属性，初始化为空字符串
    public string Description { get; set; } = string.Empty;

    # 定义 X 属性，表示 X 坐标
    public int X { get; set; }

    # 定义 Y 属性，表示 Y 坐标
    public int Y { get; set; }

    # 定义 Width 属性，表示宽度
    public int Width { get; set; }

    # 定义 Height 属性，表示高度
    public int Height { get; set; }

    # 定义 RecordDpi 属性，初始化为 1.0，表示记录的 DPI
    public double RecordDpi { get; set; } = 1.0;
}
```