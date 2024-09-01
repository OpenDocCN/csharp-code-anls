# `.\better-genshin-impact\BetterGenshinImpact\Genshin\Settings\LanguageSettings.cs`

```cs
# 定义一个命名空间 BetterGenshinImpact.Genshin.Settings
﻿namespace BetterGenshinImpact.Genshin.Settings;

# 定义一个语言设置类
public class LanguageSettings
{
    # 受保护的文本语言属性
    public TextLanguage TextLang { get; protected set; }
    # 受保护的语音语言属性
    public VoiceLanguage VoiceLang { get; protected set; }

    # 构造函数，接受一个 MainJson 对象并调用 Load 方法
    public LanguageSettings(MainJson data)
    {
        Load(data);
    }

    # 加载语言设置，根据 MainJson 对象的内容设置 TextLang 和 VoiceLang
    public void Load(MainJson data)
    {
        # 从 MainJson 对象中获取设备的文本语言类型并设置 TextLang
        TextLang = (TextLanguage)data.DeviceLanguageType;
        # 从 MainJson 对象中获取设备的语音语言类型并设置 VoiceLang
        VoiceLang = (VoiceLanguage)data.DeviceVoiceLanguageType;
    }
}

# 定义语音语言枚举类型
public enum VoiceLanguage
{
    Chinese,    # 中文
    English,    # 英文
    Japanese,   # 日文
    Korean,     # 韩文
}

# 定义文本语言枚举类型
public enum TextLanguage
{
    None,               # 无语言
    English,            # 英文
    SimplifiedChinese,  # 简体中文
    TraditionalChinese, # 繁体中文
    French,             # 法文
    German,             # 德文
    Spanish,            # 西班牙文
    Portugese,          # 葡萄牙文
    Russian,            # 俄文
    Japanese,           # 日文
    Korean,             # 韩文
    Thai,               # 泰文
    Vietnamese,         # 越南文
    Indonesian,         # 印尼文
}
```