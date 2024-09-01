# `.\better-genshin-impact\BetterGenshinImpact\Genshin\Settings\InputDataSettings.cs`

```cs
﻿using System; // 引入系统命名空间，提供基本的类和功能
using System.Diagnostics; // 引入调试命名空间，用于调试输出
using System.Text.Json; // 引入 JSON 序列化和反序列化功能
using System.Text.Json.Serialization; // 引入 JSON 属性序列化和反序列化功能

namespace BetterGenshinImpact.Genshin.Settings; // 定义命名空间

public class InputDataSettings
{
    // 存储输入数据配置的字段，初始值为 null
    public InputDataConfig? data = null;

    // 只读属性，返回 MouseSensitivity 的值
    public double MouseSensitivity => data!.MouseSensitivity;

    // 只读属性，将 MouseFocusSenseSenseIndex 转换为枚举类型
    public MouseFocusSenseIndex MouseFocusSenseIndex => (MouseFocusSenseIndex)data!.MouseFocusSenseIndex;

    // 只读属性，将 MouseFocusSenseSenseIndexY 转换为枚举类型
    public MouseFocusSenseIndex MouseFocusSenseIndexY => (MouseFocusSenseIndex)data!.MouseFocusSenseIndexY;

    // 构造函数，接收 MainJson 类型的参数并加载数据
    public InputDataSettings(MainJson data)
    {
        Load(data); // 调用 Load 方法加载数据
    }

    // 加载输入数据的方法
    public void Load(MainJson data)
    {
        try
        {
            // 如果输入数据为空或未定义，则直接返回
            if (string.IsNullOrEmpty(data.InputData))
            {
                return;
            }

            // 将 JSON 字符串反序列化为 InputDataConfig 对象
            this.data = JsonSerializer.Deserialize<InputDataConfig>(data.InputData);
        }
        catch (Exception e) // 捕获异常
        {
            // 以调试方式输出异常信息
            Debug.WriteLine(e.ToString());
        }
    }
}

public class InputDataConfig
{
    // 将 JSON 属性 "mouseSensitivity" 映射到 MouseSensitivity 属性
    [JsonPropertyName("mouseSensitivity")]
    public double MouseSensitivity { get; set; }

    // 将 JSON 属性 "mouseFocusSenseIndex" 映射到 MouseFocusSenseIndex 属性
    [JsonPropertyName("mouseFocusSenseIndex")]
    public int MouseFocusSenseIndex { get; set; }

    // 将 JSON 属性 "mouseFocusSenseIndexY" 映射到 MouseFocusSenseIndexY 属性
    [JsonPropertyName("mouseFocusSenseIndexY")]
    public int MouseFocusSenseIndexY { get; set; }
}

public enum MouseFocusSenseIndex
{
    Level1, // 定义枚举值 Level1
    Level2, // 定义枚举值 Level2
    Level3, // 定义枚举值 Level3
    Level4, // 定义枚举值 Level4
    Level5, // 定义枚举值 Level5
}
```