# `.\better-genshin-impact\BetterGenshinImpact\Core\Script\Dependence\LimitedFile.cs`

```cs
﻿using BetterGenshinImpact.Core.Script.Utils;  // 引入 BetterGenshinImpact.Core.Script.Utils 命名空间
using System;  // 引入 System 命名空间
using System.IO;  // 引入 System.IO 命名空间，用于文件操作
using System.Threading.Tasks;  // 引入 System.Threading.Tasks 命名空间，用于异步编程

namespace BetterGenshinImpact.Core.Script.Dependence;  // 定义 BetterGenshinImpact.Core.Script.Dependence 命名空间

// 定义 LimitedFile 类，构造函数接受一个 rootPath 参数
public class LimitedFile(string rootPath)
{
    /// <summary>
    /// Normalize and validate a path.
    /// </summary>
    // 规范化和验证路径
    private string NormalizePath(string path)
    {
        // 使用 ScriptUtils 的 NormalizePath 方法规范化路径
        return ScriptUtils.NormalizePath(rootPath, path);
    }

    /// <summary>
    /// Read all text from a file.
    /// </summary>
    /// <param name="path">File path.</param>
    /// <returns>Text read from file.</returns>
    // 读取文件的所有文本，路径为同步方法
    public string ReadTextSync(string path)
    {
        // 规范化文件路径
        path = NormalizePath(path);
        // 读取文件并返回内容
        return File.ReadAllText(path);
    }

    /// <summary>
    /// Read all text from a file.
    /// </summary>
    /// <param name="path">File path.</param>
    /// <returns>Text read from file.</returns>
    // 读取文件的所有文本，路径为异步方法
    public async Task<string> ReadText(string path)
    {
        // 规范化文件路径
        path = NormalizePath(path);
        // 异步读取文件内容并返回
        var ret = await File.ReadAllTextAsync(path);
        return ret;
    }

    /// <summary>
    /// Read all text from a file.
    /// </summary>
    /// <param name="path">File path.</param>
    /// <param name="callbackFunc">Callback function.</param>
    /// <returns>Text read from file.</returns>
    // 读取文件的所有文本，路径为异步方法，支持回调函数
    public async Task<string> ReadText(string path, dynamic callbackFunc)
    {
        try
        {
            // 规范化文件路径
            path = NormalizePath(path);
            // 异步读取文件内容
            var ret = await File.ReadAllTextAsync(path);
            // 调用回调函数，并传递读取的内容
            callbackFunc(null, ret);
            return ret;
        }
        catch (Exception ex)
        {
            // 如果发生异常，调用回调函数，并传递异常信息
            callbackFunc(ex.ToString(), null);
            return string.Empty;
        }
    }
}
```