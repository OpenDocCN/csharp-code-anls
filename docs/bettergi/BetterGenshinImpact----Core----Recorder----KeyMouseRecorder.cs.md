# `.\better-genshin-impact\BetterGenshinImpact\Core\Recorder\KeyMouseRecorder.cs`

```cs
# 引入相关命名空间
﻿using BetterGenshinImpact.Core.Recorder.Model; # 引入宏事件模型
using BetterGenshinImpact.GameTask; # 引入游戏任务功能
using BetterGenshinImpact.GameTask.Common; # 引入通用游戏任务功能
using BetterGenshinImpact.GameTask.Common.Map; # 引入地图相关功能
using Gma.System.MouseKeyHook; # 引入鼠标和键盘钩子功能
using SharpDX.DirectInput; # 引入 DirectInput 库
using System; # 引入基础系统功能
using System.Collections.Generic; # 引入集合类
using System.Text.Json; # 引入 JSON 序列化功能
using System.Text.Json.Serialization; # 引入 JSON 序列化设置
using System.Windows.Forms; # 引入 Windows 窗体功能

namespace BetterGenshinImpact.Core.Recorder; # 定义命名空间

# 定义 KeyMouseRecorder 类
public class KeyMouseRecorder
{
    # 定义一个只读属性，存储宏事件的列表
    public List<MacroEvent> MacroEvents { get; } = [];

    # 定义属性，记录录制开始的时间
    public DateTime StartTime { get; set; } = DateTime.UtcNow;

    # 定义属性，记录最后一次方向检测的时间
    public DateTime LastOrientationDetection { get; set; } = DateTime.UtcNow;

    # 定义属性，设置合并事件的最大时间间隔
    public double MergedEventTimeMax { get; set; } = 20.0;

    # 定义静态的 JSON 序列化选项
    public static readonly JsonSerializerOptions JsonOptions = new()
    {
        # 设置 JSON 数字处理选项，允许命名的浮点字面量
        NumberHandling = JsonNumberHandling.AllowNamedFloatingPointLiterals,
        # 设置 JSON 编码器，允许放宽的 JSON 转义
        Encoder = System.Text.Encodings.Web.JavaScriptEncoder.UnsafeRelaxedJsonEscaping,
        # 设置 JSON 属性命名策略为驼峰式
        PropertyNamingPolicy = JsonNamingPolicy.CamelCase,
        # 允许 JSON 末尾逗号
        AllowTrailingCommas = true,
        # 跳过 JSON 注释处理
        ReadCommentHandling = JsonCommentHandling.Skip,
        # 在序列化时忽略空值属性
        DefaultIgnoreCondition = JsonIgnoreCondition.WhenWritingNull
    };

    # 方法 ToJsonMacro 还未完成
    public string ToJsonMacro()
    {
        // 获取系统信息中的捕获区域矩形
        var rect = TaskContext.Instance().SystemInfo.CaptureAreaRect;
        // 初始化合并后的宏事件列表
        var mergedMacroEvents = new List<MacroEvent>();
        // 当前正在合并的宏事件
        MacroEvent? currentMerge = null;
        // 遍历所有宏事件
        foreach (var macroEvent in MacroEvents)
        {
            // 如果当前没有合并事件，则设置为当前事件
            if (currentMerge == null)
            {
                currentMerge = macroEvent;
                continue;
            }
            // 如果当前事件与正在合并的事件类型不同，则添加到结果列表中
            if (currentMerge.Type != macroEvent.Type)
            {
                mergedMacroEvents.Add(currentMerge);
                currentMerge = macroEvent;
                continue;
            }
            // 根据宏事件的类型进行处理
            switch (macroEvent.Type)
            {
                case MacroEventType.MouseMoveTo:
                    // 控制合并时间片段长度
                    if (macroEvent.Time - currentMerge.Time > MergedEventTimeMax)
                    {
                        mergedMacroEvents.Add(currentMerge);
                        currentMerge = macroEvent;
                        break;
                    }
                    // 合并为最后一个事件的位置，避免丢步
                    currentMerge.MouseX = macroEvent.MouseX;
                    currentMerge.MouseY = macroEvent.MouseY;
                    break;

                case MacroEventType.MouseMoveBy:
                    // 控制合并时间片段长度
                    if (macroEvent.Time - currentMerge.Time > MergedEventTimeMax)
                    {
                        mergedMacroEvents.Add(currentMerge);
                        currentMerge = macroEvent;
                        break;
                    }
                    // 相对位移量相加
                    currentMerge.MouseX += macroEvent.MouseX;
                    currentMerge.MouseY += macroEvent.MouseY;
                    if (macroEvent.CameraOrientation != null)
                    {
                        currentMerge.CameraOrientation = macroEvent.CameraOrientation;
                    }
                    break;

                default:
                    // 对于其他事件，直接添加到结果列表
                    mergedMacroEvents.Add(currentMerge);
                    mergedMacroEvents.Add(macroEvent);
                    currentMerge = null;
                    break;
            }
        }
        // 创建一个新的键鼠脚本对象
        KeyMouseScript keyMouseScript = new()
        {
            MacroEvents = mergedMacroEvents,
            Info = new KeyMouseScriptInfo
            {
                X = rect.X,
                Y = rect.Y,
                Width = rect.Width,
                Height = rect.Height,
                RecordDpi = TaskContext.Instance().DpiScale
            }
        };
        // 将键鼠脚本对象序列化为 JSON 字符串
        return JsonSerializer.Serialize(keyMouseScript, JsonOptions);
    }

    // 处理键盘按下事件
    public void KeyDown(KeyEventArgs e)
    {
        MacroEvents.Add(new MacroEvent
        {
            Type = MacroEventType.KeyDown,
            KeyCode = e.KeyValue,
            Time = (DateTime.UtcNow - StartTime).TotalMilliseconds
        });
    }

    // 处理键盘释放事件
    # 向宏事件列表中添加一个新事件，表示键盘按键抬起的动作
    MacroEvents.Add(new MacroEvent
    {
        # 事件类型为 KeyUp（按键抬起）
        Type = MacroEventType.KeyUp,
        # 记录按键的虚拟键码
        KeyCode = e.KeyValue,
        # 记录从开始时间到当前时间的毫秒数
        Time = (DateTime.UtcNow - StartTime).TotalMilliseconds
    });

    # 处理鼠标按下事件
    public void MouseDown(MouseEventExtArgs e)
    {
        # 向宏事件列表中添加一个新事件，表示鼠标按下的动作
        MacroEvents.Add(new MacroEvent
        {
            # 事件类型为 MouseDown（鼠标按下）
            Type = MacroEventType.MouseDown,
            # 记录鼠标指针的 X 坐标
            MouseX = e.X,
            # 记录鼠标指针的 Y 坐标
            MouseY = e.Y,
            # 记录鼠标按钮的名称
            MouseButton = e.Button.ToString(),
            # 记录从开始时间到当前时间的毫秒数
            Time = (DateTime.UtcNow - StartTime).TotalMilliseconds
        });
    }

    # 处理鼠标释放事件
    public void MouseUp(MouseEventExtArgs e)
    {
        # 向宏事件列表中添加一个新事件，表示鼠标释放的动作
        MacroEvents.Add(new MacroEvent
        {
            # 事件类型为 MouseUp（鼠标释放）
            Type = MacroEventType.MouseUp,
            # 记录鼠标指针的 X 坐标
            MouseX = e.X,
            # 记录鼠标指针的 Y 坐标
            MouseY = e.Y,
            # 记录鼠标按钮的名称
            MouseButton = e.Button.ToString(),
            # 记录从开始时间到当前时间的毫秒数
            Time = (DateTime.UtcNow - StartTime).TotalMilliseconds
        });
    }

    # 处理鼠标移动到指定位置的事件
    public void MouseMoveTo(MouseEventExtArgs e)
    {
        # 向宏事件列表中添加一个新事件，表示鼠标移动到某个位置的动作
        MacroEvents.Add(new MacroEvent
        {
            # 事件类型为 MouseMoveTo（鼠标移动到）
            Type = MacroEventType.MouseMoveTo,
            # 记录鼠标指针的 X 坐标
            MouseX = e.X,
            # 记录鼠标指针的 Y 坐标
            MouseY = e.Y,
            # 记录从开始时间到当前时间的毫秒数
            Time = (DateTime.UtcNow - StartTime).TotalMilliseconds
        });
    }

    # 处理鼠标相对移动的事件
    public void MouseMoveBy(MouseState state)
    {
        # 获取当前时间
        var now = DateTime.UtcNow;
        # 定义一个可为空的整数变量，用于存储摄像头方向
        int? cao = null;
        # 检查是否记录摄像头方向
        if (TaskContext.Instance().Config.RecordConfig.IsRecordCameraOrientation)
        {
            # 如果距离上次检测已经超过 100 毫秒，则重新计算摄像头方向
            if ((now - LastOrientationDetection).TotalMilliseconds > 100.0)
            {
                # 更新上次方向检测时间
                LastOrientationDetection = now;
                # 计算当前摄像头方向
                cao = CameraOrientation.Compute(TaskControl.CaptureToRectArea().SrcGreyMat);
            }
        }
        # 向宏事件列表中添加一个新事件，表示鼠标相对移动的动作
        MacroEvents.Add(new MacroEvent
        {
            # 事件类型为 MouseMoveBy（鼠标相对移动）
            Type = MacroEventType.MouseMoveBy,
            # 记录鼠标指针的 X 坐标
            MouseX = state.X,
            # 记录鼠标指针的 Y 坐标
            MouseY = state.Y,
            # 记录从开始时间到当前时间的毫秒数
            Time = (now - StartTime).TotalMilliseconds,
            # 记录摄像头方向（如果有）
            CameraOrientation = cao,
        });
    }
# 这是一个结束大括号，通常用于标记代码块的结束
}
```