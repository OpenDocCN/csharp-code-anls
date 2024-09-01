# `.\better-genshin-impact\BetterGenshinImpact\Core\Script\Utils\ScriptUtils.cs`

```cs
﻿using System;
using System.IO;

namespace BetterGenshinImpact.Core.Script.Utils;

public class ScriptUtils
{
    /// <summary>
    /// Normalize and validate a path.
    /// </summary>
    // 将给定的路径标准化并验证其有效性
    public static string NormalizePath(string root, string path)
    {
        // 将路径中的反斜杠替换为正斜杠，以统一路径格式
        path = path.Replace('\\', '/');
        // 获取从根目录开始的完整路径
        var fullPath = Path.GetFullPath(Path.Combine(root, path));

        // 如果完整路径不以根目录开头，则抛出异常，防止路径越界
        if (!fullPath.StartsWith(root))
        {
            throw new ArgumentException($"Path '{path}' is not allowed, because its outside the caged root folder!");
        }

        // 返回标准化后的完整路径
        return fullPath;
    }
}
```