# `.\better-genshin-impact\Build\MicaSetup\Helper\Setup\PackHelper.cs`

```cs
# 引入必要的命名空间
﻿using System;
using System.Collections.Generic;
using System.IO;
using System.Reflection;
using System.Text;

namespace MicaSetup.Helper;

# 定义一个静态帮助类
public static class PackHelper
{
    # 获取程序集的标题属性
    public static string AssemblyTitle => GetAssemblyTitle(typeof(Option).Assembly);
    # 获取程序集的版权属性
    public static readonly string AssemblyCopyright = GetAssemblyCopyright(typeof(Option).Assembly);
    # 获取程序集的版本信息
    public static readonly string AssemblyVersion = GetAssemblyVersion(typeof(Option).Assembly);

    # 获取程序集标题
    public static string GetAssemblyTitle(Assembly assembly = null!)
    {
        # 从程序集获取标题属性
        AssemblyTitleAttribute attr = GetAssembly<AssemblyTitleAttribute>(assembly);
        # 返回标题，如果标题为空，则返回程序集的文件名
        return attr?.Title ?? Path.GetFileNameWithoutExtension((assembly ?? Assembly.GetExecutingAssembly()).CodeBase);
    }

    # 获取程序集版权信息
    public static string GetAssemblyCopyright(Assembly assembly = null!)
        => GetAssembly<AssemblyCopyrightAttribute>(assembly)?.Copyright!;

    # 定义版本类型标志
    [Flags]
    private enum EVersionType
    {
        Major = 0,
        Minor = 1,
        Build = 2,
        Revision = 4,
    }

    # 获取程序集版本信息的指定部分
    private static string GetAssemblyVersion(this Assembly assembly, EVersionType type = EVersionType.Major | EVersionType.Minor | EVersionType.Build)
    {
        # 获取程序集的版本信息
        Version version = assembly.GetName().Version;
        StringBuilder sb = new();

        # 如果包含主版本号，添加到结果中
        if (type.HasFlag(EVersionType.Major))
        {
            sb.Append(version.Major);
        }
        # 如果包含次版本号，添加到结果中
        if (type.HasFlag(EVersionType.Minor))
        {
            if (sb.Length > 0)
                sb.Append(".");
            sb.Append(version.Minor);
        }
        # 如果包含构建号，添加到结果中
        if (type.HasFlag(EVersionType.Build))
        {
            if (sb.Length > 0)
                sb.Append(".");
            sb.Append(version.Build);
        }
        # 如果包含修订号，添加到结果中
        if (type.HasFlag(EVersionType.Revision))
        {
            if (sb.Length > 0)
                sb.Append(".");
            sb.Append(version.Revision);
        }
        # 返回版本号字符串
        return sb.ToString();
    }

    # 获取指定类型的程序集特性
    private static TAssy GetAssembly<TAssy>(Assembly assembly = null!)
    {
        TAssy[] assemblies = GetAssemblies<TAssy>(assembly);

        # 如果找到特性，返回第一个
        if (assemblies.Length > 0)
        {
            return assemblies[0];
        }
        return default!;
    }

    # 获取指定类型的所有程序集特性
    private static TAssy[] GetAssemblies<TAssy>(Assembly assembly = null!)
    {
        # 获取程序集的所有自定义特性
        object[] attributes = (assembly ?? Assembly.GetExecutingAssembly()).GetCustomAttributes(typeof(TAssy), false);
        List<TAssy> attributeList = new();

        # 过滤并收集指定类型的特性
        foreach (object attribute in attributes)
        {
            if (attribute is TAssy assy)
            {
                attributeList.Add(assy);
            }
        }
        # 返回特性数组
        return attributeList.ToArray();
    }
}
```