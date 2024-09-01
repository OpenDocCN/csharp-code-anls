# `.\better-genshin-impact\BetterGenshinImpact\Core\Recorder\Model\MacroEvent.cs`

```cs
# 引入 System 命名空间
﻿using System;

# 定义命名空间
namespace BetterGenshinImpact.Core.Recorder.Model;

# 标记该类为可序列化
[Serializable]
# 定义宏事件类
public class MacroEvent
{
    # 定义宏事件类型属性
    public MacroEventType Type { get; set; }
    # 定义可选的按键代码属性
    public int? KeyCode { get; set; }
    # 定义鼠标 X 坐标属性
    public int MouseX { get; set; }
    # 定义鼠标 Y 坐标属性
    public int MouseY { get; set; }
    # 定义可选的鼠标按钮属性
    public string? MouseButton { get; set; }
    # 定义时间属性
    public double Time { get; set; }

    # 定义可选的相机方向属性
    public int? CameraOrientation { get; set; }
}

# 定义宏事件类型枚举
public enum MacroEventType
{
    # 键盘按下事件
    KeyDown,
    # 键盘抬起事件
    KeyUp,
    # 鼠标移动到指定位置事件
    MouseMoveTo,
    # 鼠标相对移动事件
    MouseMoveBy,
    # 鼠标按下事件
    MouseDown,
    # 鼠标抬起事件
    MouseUp
}
```