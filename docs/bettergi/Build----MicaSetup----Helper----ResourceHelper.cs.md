# `.\better-genshin-impact\Build\MicaSetup\Helper\ResourceHelper.cs`

```cs
﻿using System;
using System.IO;
using System.IO.Packaging;
using System.Reflection;
using System.Text;
using System.Windows;
using System.Windows.Resources;

namespace MicaSetup.Helper;

public static class ResourceHelper
{
    // 静态构造函数，用于初始化类的静态成员
    static ResourceHelper()
    {
        // 检查 "pack" 是否已知的 URI 方案，如果未注册，则初始化它
        if (!UriParser.IsKnownScheme("pack"))
            _ = PackUriHelper.UriSchemePack;
    }

    // 检查指定的 URI 是否指向一个资源，如果可以成功打开流则返回 true，否则返回 false
    public static bool HasResource(string uriString)
    {
        try
        {
            // 尝试获取资源流
            using Stream stream = GetStream(uriString);
            // 读取流（此处仅用来确认流是否可用）
            _ = stream;
            return true;
        }
        catch
        {
            // 捕获异常但不处理
        }
        // 如果发生异常或资源不可用，则返回 false
        return false;
    }

    // 从指定的 URI 获取资源内容并以字节数组形式返回
    public static byte[] GetBytes(string uriString)
    {
        // 创建 URI 对象
        Uri uri = new(uriString);
        // 获取资源流信息
        StreamResourceInfo info = Application.GetResourceStream(uri);
        // 使用二进制读取器读取资源流内容
        using BinaryReader stream = new(info.Stream);
        // 返回资源流内容的字节数组
        return stream.ReadBytes((int)info.Stream.Length);
    }

    // 从指定的 URI 获取资源流
    public static Stream GetStream(string uriString)
    {
        // 创建 URI 对象
        Uri uri = new(uriString);
        // 获取资源流信息
        StreamResourceInfo info = Application.GetResourceStream(uri);
        // 返回资源流（如果存在的话）
        return info?.Stream!;
    }

    // 从指定的 URI 获取资源内容并以字符串形式返回，使用指定的编码（默认为 UTF-8）
    public static string GetString(string uriString, Encoding encoding = null!)
    {
        // 创建 URI 对象
        Uri uri = new(uriString);
        // 获取资源流信息
        StreamResourceInfo info = Application.GetResourceStream(uri);
        // 使用指定编码创建流读取器，读取并返回资源内容
        using StreamReader stream = new(info.Stream, encoding ?? Encoding.UTF8);
        return stream.ReadToEnd();
    }

    // 从指定的 URI 获取资源字典并返回指定的键的值
    public static object GetResourceDictionary<T>(string uriString, string key) where T : class
    {
        // 创建资源字典对象并设置源 URI
        ResourceDictionary rd = new()
        {
            Source = new Uri(uriString),
        };
        // 返回资源字典中指定键的值，强制转换为指定类型
        return (rd[key] as T)!;
    }

    // 从指定的程序集获取嵌入的清单资源流
    public static Stream GetManifestResourceStream(string name, Assembly assembly = null!)
    {
        // 获取指定程序集的嵌入资源流（默认为当前执行的程序集）
        Stream stream = (assembly ?? Assembly.GetExecutingAssembly()).GetManifestResourceStream(name);
        return stream;
    }

    // 从指定的程序集获取嵌入的清单资源内容并以字节数组形式返回
    public static byte[] GetManifestResourceBytes(string name, Assembly assembly = null!)
    {
        // 获取资源流
        using Stream stream = GetManifestResourceStream(name, assembly);
        // 使用二进制读取器读取资源流内容
        using BinaryReader reader = new(stream);
        // 返回资源流内容的字节数组
        return reader.ReadBytes((int)stream.Length);
    }
}
```