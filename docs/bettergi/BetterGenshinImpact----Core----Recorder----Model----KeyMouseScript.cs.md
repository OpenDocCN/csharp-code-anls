# `.\better-genshin-impact\BetterGenshinImpact\Core\Recorder\Model\KeyMouseScript.cs`

```cs
# 引入系统基础类库
﻿using System;
# 引入集合类库
using System.Collections.Generic;
# 引入调试类库
using System.Diagnostics;
# 引入 PInvoke 库
using Vanara.PInvoke;

# 定义命名空间
namespace BetterGenshinImpact.Core.Recorder.Model;

# 指示该类可以被序列化
[Serializable]
# 定义一个公共类 KeyMouseScript
public class KeyMouseScript
{
    # 定义一个公共属性，用于存储宏事件列表，初始化为空列表
    public List<MacroEvent> MacroEvents { get; set; } = [];
    # 定义一个公共可空属性，用于存储脚本信息
    public KeyMouseScriptInfo? Info { get; set; }

    # 定义一个公共方法，用于将原始脚本转换为适应当前分辨率的脚本
    /// <summary>
    /// 转换原始脚本为适应当前分辨率的脚本
    /// </summary>
    public void Adapt(RECT captureRect, double dpiScale)
    {
        # 遍历宏事件列表中的每个宏事件
        foreach (var macroEvent in MacroEvents)
        {
            # 如果 Info 为 null 或者其宽度或高度为 0，则输出错误信息并退出
            if (Info == null || Info.Width == 0 || Info.Height == 0)
            {
                Debug.WriteLine("错误的脚本数据 Info.Width == 0 || Info.Height == 0");
                break;
            }

            # 如果宏事件类型是鼠标移动到、鼠标按下或鼠标抬起
            if (macroEvent.Type == MacroEventType.MouseMoveTo
                || macroEvent.Type == MacroEventType.MouseDown
                || macroEvent.Type == MacroEventType.MouseUp)
            {
                # 计算适应当前分辨率后的鼠标 X 坐标
                macroEvent.MouseX = (int)(captureRect.X + (macroEvent.MouseX - Info.X) * captureRect.Width * 1d / Info.Width);
                # 计算适应当前分辨率后的鼠标 Y 坐标
                macroEvent.MouseY = (int)(captureRect.Y + (macroEvent.MouseY - Info.Y) * captureRect.Height * 1d / Info.Height);
            }
            # 如果宏事件类型是鼠标相对移动
            else if (macroEvent.Type == MacroEventType.MouseMoveBy)
            {
                # 计算根据 DPI 缩放调整后的鼠标 X 坐标
                macroEvent.MouseX = (int)Math.Round(macroEvent.MouseX / Info.RecordDpi * dpiScale, 0);
                # 计算根据 DPI 缩放调整后的鼠标 Y 坐标
                macroEvent.MouseY = (int)Math.Round(macroEvent.MouseY / Info.RecordDpi * dpiScale, 0);
            }
        }
    }
}
```