# `.\better-genshin-impact\Build\MicaSetup\Helper\CommandLineHelper.cs`

```cs
# 引入必需的命名空间
﻿using System;
using System.Collections.Specialized;
using System.Linq;
using System.Text.RegularExpressions;

namespace MicaSetup.Helper;

# 定义静态类 CommandLineHelper
public static class CommandLineHelper
{
    # 定义一个公开的静态属性 Values，用于存储命令行参数的键值对
    public static StringDictionary Values { get; private set; } = new();

    # 静态构造函数，用于初始化 CommandLineHelper 类
    static CommandLineHelper()
    {
        # 获取命令行参数数组，包含应用程序名称和传递的参数
        string[] args = Environment.GetCommandLineArgs();
        
        # 创建用于分割命令行参数的正则表达式
        Regex spliter = new(@"^-{1,2}|^/|=|:", RegexOptions.IgnoreCase | RegexOptions.Compiled);
        
        # 创建用于去除参数值引号的正则表达式
        Regex remover = new(@"^['""]?(.*?)['""]?$", RegexOptions.IgnoreCase | RegexOptions.Compiled);

        # 初始化临时变量用于存储当前参数名和参数值
        string param = null!;
        string[] parts;

        # 遍历命令行参数，跳过第一个参数（通常是程序名）
        foreach (string txt in args.Skip(1))
        {
            # 使用正则表达式分割参数字符串
            parts = spliter.Split(txt, 3);

            # 根据分割后的部分数目处理不同情况
            switch (parts.Length)
            {
                # 只有一个部分，表示当前参数没有值
                case 1:
                    if (param != null)
                    {
                        # 如果之前存在参数名，且尚未添加到字典中，则将其值设为当前部分的值
                        if (!Values.ContainsKey(param))
                        {
                            parts[0] = remover.Replace(parts[0], "$1");
                            Values.Add(param, parts[0]);
                        }
                        # 重置参数名
                        param = null!;
                    }
                    break;

                # 两个部分，表示当前参数是一个布尔型开关
                case 2:
                    if (param != null)
                    {
                        # 如果之前存在参数名，且尚未添加到字典中，则将其值设为“true”
                        if (!Values.ContainsKey(param))
                        {
                            Values.Add(param, "true");
                        }
                    }
                    # 更新当前参数名
                    param = parts[1];
                    break;

                # 三个部分，表示当前参数是一个键值对
                case 3:
                    if (param != null)
                    {
                        # 如果之前存在参数名，且尚未添加到字典中，则将其值设为“true”
                        if (!Values.ContainsKey(param))
                        {
                            Values.Add(param, "true");
                        }
                    }
                    # 更新当前参数名
                    param = parts[1];
                    if (!Values.ContainsKey(param))
                    {
                        # 去除值的引号，并将其添加到字典中
                        parts[2] = remover.Replace(parts[2], "$1");
                        Values.Add(param, parts[2]);
                    }
                    # 重置参数名
                    param = null!;
                    break;
            }
        }
        # 如果循环结束时还有未处理的参数名，则将其值设为“True”
        if (param != null)
        {
            if (!Values.ContainsKey(param))
            {
                Values.Add(param, bool.TrueString);
            }
        }
    }

    # 检查字典中是否包含指定的键
    public static bool Has(string key) => Values.ContainsKey(key);

    # 根据指定的键获取布尔值，若失败则返回 null
    public static bool? GetValueBoolean(string key)
    {
        bool? ret = null;

        try
        {
            # 获取对应的值并尝试将其转换为布尔值
            string value = Values[key];

            if (!string.IsNullOrEmpty(value))
            {
                ret = Convert.ToBoolean(value);
            }
        }
        catch
        {
            # 捕捉任何异常但不做处理
        }
        return ret;
    }

    # 根据指定的键获取 32 位整数值
    public static int? GetValueInt32(string key)
    # 声明一个可空的整型变量，初始化为 null
    int? ret = null;

    try
    {
        # 从 Values 字典中获取指定键的值
        string value = Values[key];

        # 如果值不为空或不为 null，将其转换为整型
        if (!string.IsNullOrEmpty(value))
        {
            ret = Convert.ToInt32(value);
        }
    }
    catch
    {
        # 捕获异常，但不进行处理
    }
    # 返回转换后的整型值或 null
    return ret;
    # 定义一个静态方法，返回可空的双精度浮点型值
    public static double? GetValueDouble(string key)
    {
        # 声明一个可空的双精度浮点型变量，初始化为 null
        double? ret = null;

        try
        {
            # 从 Values 字典中获取指定键的值
            string value = Values[key];

            # 如果值不为空或不为 null，将其转换为双精度浮点型
            if (!string.IsNullOrEmpty(value))
            {
                ret = Convert.ToDouble(value);
            }
        }
        catch
        {
            # 捕获异常，但不进行处理
        }
        # 返回转换后的双精度浮点型值或 null
        return ret;
    }

    # 定义一个静态方法，检查指定键的布尔值
    public static bool IsValueBoolean(string key)
    {
        # 调用 GetValueBoolean 方法，如果返回 null，则默认返回 false
        return GetValueBoolean(key) ?? false;
    }
# 结束当前代码块或代码块的某个部分，具体取决于上下文
}
```