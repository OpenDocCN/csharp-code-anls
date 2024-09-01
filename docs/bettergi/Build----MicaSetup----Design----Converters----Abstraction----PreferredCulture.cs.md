# `.\better-genshin-impact\Build\MicaSetup\Design\Converters\Abstraction\PreferredCulture.cs`

```cs
# 引入 System.Globalization 命名空间以支持文化相关功能
﻿using System.Globalization;

# 定义 MicaSetup.Design.Converters 命名空间
namespace MicaSetup.Design.Converters;

# 定义一个枚举类型 PreferredCulture
public enum PreferredCulture
{
    # <summary>
    # 使用由 <seealso cref="IValueConverter"/> 提供的默认文化
    # </summary>
    ConverterCulture,

    # <summary>
    # 使用 <seealso cref="CultureInfo.CurrentCulture"/> 替代默认转换器文化
    # </summary>
    CurrentCulture,

    # <summary>
    # 使用 <seealso cref="CultureInfo.CurrentUICulture"/> 替代默认转换器文化
    # </summary>
    CurrentUICulture,
}
```