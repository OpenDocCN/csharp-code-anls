# `.\better-genshin-impact\BetterGenshinImpact\Helpers\ResourceHelper.cs`

```cs
# 引入必要的系统和 WPF 命名空间
﻿using System;
using System.IO;
using System.IO.Packaging;
using System.Text;
using System.Windows;
using System.Windows.Resources;

# 定义命名空间
namespace BetterGenshinImpact.Helpers;

# 定义静态类 ResourceHelper
internal static class ResourceHelper
{
    # 静态构造函数
    static ResourceHelper()
    {
        # 如果 URI 解析器中未注册 "pack" 方案，则注册该方案
        if (!UriParser.IsKnownScheme("pack"))
            _ = PackUriHelper.UriSchemePack;
    }

    # 根据 URI 字符串获取字节数组
    public static byte[] GetBytes(string uriString)
    {
        # 创建 URI 实例
        Uri uri = new(uriString);
        # 获取指定 URI 对应的资源流信息
        StreamResourceInfo? info = Application.GetResourceStream(uri);
        # 使用 BinaryReader 读取资源流内容，并返回字节数组
        using BinaryReader stream = new(info.Stream);
        return stream.ReadBytes((int)info.Stream.Length);
    }

    # 根据 URI 字符串获取资源流
    public static Stream GetStream(string uriString)
    {
        # 创建 URI 实例
        Uri uri = new(uriString);
        # 获取指定 URI 对应的资源流信息
        StreamResourceInfo? info = Application.GetResourceStream(uri);
        # 返回资源流
        return info?.Stream!;
    }

    # 根据 URI 字符串获取字符串内容，支持指定编码
    public static string GetString(string uriString, Encoding encoding = null!)
    {
        # 创建 URI 实例
        Uri uri = new(uriString);
        # 获取指定 URI 对应的资源流信息，若找不到则抛出异常
        StreamResourceInfo? info = Application.GetResourceStream(uri) ?? throw new FileNotFoundException($"Resource not found: {uriString}");
        # 使用 StreamReader 读取资源流内容，并返回字符串
        using StreamReader stream = new(info.Stream, encoding ?? Encoding.UTF8);
        return stream.ReadToEnd();
    }
}
```