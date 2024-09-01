# `.\better-genshin-impact\Build\MicaSetup\Design\Converters\Abstraction\ValueConvertersConfig.cs`

```cs
# 定义命名空间 MicaSetup.Design.Converters
﻿namespace MicaSetup.Design.Converters;

# 定义一个公共静态类 ValueConvertersConfig
public static class ValueConvertersConfig
{
    # 定义一个公共静态属性 DefaultPreferredCulture
    # 用于获取或设置默认首选文化，初始值为 PreferredCulture.ConverterCulture
    public static PreferredCulture DefaultPreferredCulture { get; set; } = PreferredCulture.ConverterCulture;
}
```