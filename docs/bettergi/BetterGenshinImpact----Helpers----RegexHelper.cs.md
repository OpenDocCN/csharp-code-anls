# `.\better-genshin-impact\BetterGenshinImpact\Helpers\RegexHelper.cs`

```cs
# 导入正则表达式相关的命名空间
﻿using System.Text.RegularExpressions;

# 定义 BetterGenshinImpact.Helpers 命名空间
namespace BetterGenshinImpact.Helpers;

# 定义一个内部静态的部分类 RegexHelper
internal static partial class RegexHelper
{
    # 使用生成的正则表达式来排除非数字字符
    [GeneratedRegex(@"[^0-9]+")]
    # 定义一个公共的部分方法，返回一个排除数字的正则表达式
    public static partial Regex ExcludeNumberRegex();

    # 使用生成的正则表达式来匹配完全由数字组成的字符串
    [GeneratedRegex(@"^[0-9]+$")]
    # 定义一个公共的部分方法，返回一个全数字的正则表达式
    public static partial Regex FullNumberRegex();
}
```