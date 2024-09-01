# `.\better-genshin-impact\BetterGenshinImpact\Core\Recognition\ONNX\BgiSessionOption.cs`

```cs
﻿using BetterGenshinImpact.GameTask; // 引入游戏任务相关的命名空间
using BetterGenshinImpact.Model; // 引入模型相关的命名空间
using Microsoft.ML.OnnxRuntime; // 引入 ONNX Runtime 命名空间
using System.ComponentModel; // 引入组件模型命名空间

namespace BetterGenshinImpact.Core.Recognition.ONNX; // 定义命名空间

public class BgiSessionOption : Singleton<BgiSessionOption> // 定义 BgiSessionOption 类，继承自 Singleton 类
{
    public static string[] InferenceDeviceTypes { get; } = ["CPU", "GPU_DirectML"]; // 定义推理设备类型数组，包含 CPU 和 GPU_DirectML

    public SessionOptions Options { get; set; } = TaskContext.Instance().Config.InferenceDevice switch // 根据推理设备类型选择 SessionOptions 实例
    {
        "CPU" => new SessionOptions(), // 如果设备是 CPU，创建默认的 SessionOptions 实例
        "GPU_DirectML" => MakeSessionOptionWithDirectMlProvider(), // 如果设备是 GPU_DirectML，调用方法创建带 DirectML 提供者的 SessionOptions 实例
        _ => throw new InvalidEnumArgumentException("无效的推理设备") // 如果设备类型无效，抛出异常
    };

    public static SessionOptions MakeSessionOptionWithDirectMlProvider() // 定义创建带 DirectML 提供者的 SessionOptions 实例的方法
    {
        var sessionOptions = new SessionOptions(); // 创建 SessionOptions 实例
        sessionOptions.AppendExecutionProvider_DML(0); // 添加 DirectML 执行提供者
        return sessionOptions; // 返回配置好的 SessionOptions 实例
    }

    // /// <summary>
    // /// 重新加载每个推理器（测试没用，只能重启）
    // /// </summary>
    // public void RefreshInference() // 定义重新加载推理器的方法（目前无效，只能重启）
    // {
    //     // 自动秘境每次都会NEW不用管
    //     // Yap、自动钓鱼
    //     GameTaskManager.RefreshTriggerConfigs(); // 刷新触发器配置
    // }
}
```