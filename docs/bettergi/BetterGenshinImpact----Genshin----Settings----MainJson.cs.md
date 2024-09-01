# `.\better-genshin-impact\BetterGenshinImpact\Genshin\Settings\MainJson.cs`

```cs
# 使用 System.Text.Json.Serialization 命名空间中的功能
﻿using System.Text.Json.Serialization;

# 定义命名空间为 BetterGenshinImpact.Genshin.Settings
namespace BetterGenshinImpact.Genshin.Settings;

# 定义一个密封类 MainJson
public sealed class MainJson
{
    # 属性 DeviceLanguageType 的 JSON 属性名为 "deviceLanguageType"
    [JsonPropertyName("deviceLanguageType")]
    public int DeviceLanguageType { get; set; }

    # 属性 DeviceVoiceLanguageType 的 JSON 属性名为 "deviceVoiceLanguageType"
    [JsonPropertyName("deviceVoiceLanguageType")]
    public int DeviceVoiceLanguageType { get; set; }

    # 属性 InputData 的 JSON 属性名为 "inputData"
    [JsonPropertyName("inputData")]
    public string? InputData { get; set; }

    # 属性 OverrideControllerMapKeyList 的 JSON 属性名为 "_overrideControllerMapKeyList"
    [JsonPropertyName("_overrideControllerMapKeyList")]
    public string[]? OverrideControllerMapKeyList { get; set; }

    # 属性 OverrideControllerMapValueList 的 JSON 属性名为 "_overrideControllerMapValueList"
    [JsonPropertyName("_overrideControllerMapValueList")]
    public string[]? OverrideControllerMapValueList { get; set; }
}
```