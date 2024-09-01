# `.\better-genshin-impact\Build\MicaSetup\Helper\System\SpecialPathHelper.cs`

```cs
# 定义命名空间和类
﻿using System;
using System.IO;
using System.Reflection;

namespace MicaSetup.Helper;

public static class SpecialPathHelper
{
    # 定义一个默认的应用数据文件夹路径
    private static string _defaultApplicationDataFolder = Option.Current.KeyName ?? Assembly.GetExecutingAssembly().GetName().Name;
    # 定义一个只读的本地应用数据路径
    private static readonly string _localApplicationData = Environment.GetFolderPath(Environment.SpecialFolder.MyDocuments);

    # 定义一个临时路径属性，组合了临时目录和随机生成的文件名
    public static string TempPath { get; } = Path.Combine(Path.GetTempPath(), Path.GetRandomFileName());

    # 设置应用数据文件夹的路径
    public static void SetApplicationDataFolder(string applicationDataFolder)
    {
        _defaultApplicationDataFolder = applicationDataFolder;
    }

    # 获取指定文件夹的完整路径，默认为默认的应用数据文件夹
    public static string GetFolder(string optionFolder = null!)
    {
        return Path.Combine(_localApplicationData, optionFolder ?? _defaultApplicationDataFolder);
    }

    # 获取指定基名的完整路径，如果目录不存在则创建
    public static string GetPath(string baseName)
    {
        # 组合配置路径
        string configPath = Path.Combine(GetFolder(), baseName);

        # 如果目录不存在则创建目录
        if (!Directory.Exists(new FileInfo(configPath).DirectoryName))
        {
            _ = Directory.CreateDirectory(new FileInfo(configPath).DirectoryName!);
        }
        # 返回配置路径
        return configPath;
    }
}

# 定义一个扩展类
public static class SpecialPathExtension
{
    # 确保指定路径的目录存在，如果不存在则创建
    public static string SureDirectoryExists(this string path)
    {
        # 如果目录不存在则创建目录
        if (!Directory.Exists(path))
        {
            Directory.CreateDirectory(path);
        }
        # 返回路径
        return path;
    }
}
```