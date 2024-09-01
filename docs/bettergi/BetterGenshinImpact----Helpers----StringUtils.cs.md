# `.\better-genshin-impact\BetterGenshinImpact\Helpers\StringUtils.cs`

```cs
﻿using System.Text; // 引入用于字符串操作的 StringBuilder 类
using System.Text.RegularExpressions; // 引入用于正则表达式操作的 Regex 类

namespace BetterGenshinImpact.Helpers; // 定义命名空间 BetterGenshinImpact.Helpers

public partial class StringUtils // 定义 StringUtils 类的一部分
{
    /// <summary>
    ///  删除所有空字符串
    /// </summary>
    /// <param name="str"></param>
    /// <returns></returns>
    public static string RemoveAllSpace(string str) // 定义一个静态方法，删除字符串中的所有空格和制表符
    {
        if (string.IsNullOrEmpty(str)) // 如果输入的字符串为空或为 null
        {
            return str; // 直接返回原字符串
        }

        return new StringBuilder(str) // 使用 StringBuilder 创建字符串的可变副本
            .Replace(" ", "") // 替换所有空格为无字符
            .Replace("\t", "") // 替换所有制表符为无字符
            .ToString(); // 转换回字符串并返回
    }

    /// <summary>
    ///  删除所有换行符
    /// </summary>
    /// <param name="str"></param>
    /// <returns></returns>
    public static string RemoveAllEnter(string str) // 定义一个静态方法，删除字符串中的所有换行符
    {
        if (string.IsNullOrEmpty(str)) // 如果输入的字符串为空或为 null
        {
            return str; // 直接返回原字符串
        }

        return new StringBuilder(str) // 使用 StringBuilder 创建字符串的可变副本
            .Replace("\n", "") // 替换所有换行符为无字符
            .Replace("\r", "") // 替换所有回车符为无字符
            .ToString(); // 转换回字符串并返回
    }

    /// <summary>
    /// 判断字符串是否是中文
    /// </summary>
    public static bool IsChinese(string str) // 定义一个静态方法，判断字符串是否全为中文
    {
        if (string.IsNullOrEmpty(str)) // 如果输入的字符串为空或为 null
        {
            return false; // 返回 false，因为空字符串不是中文
        }

        return ChineseRegex().IsMatch(str); // 使用正则表达式检查字符串是否全为中文
    }

    /// <summary>
    /// 保留中文字符
    /// </summary>
    /// <param name="str"></param>
    /// <returns></returns>
    public static string ExtractChinese(string str) // 定义一个静态方法，提取字符串中的所有中文字符
    {
        //声明存储结果的字符串
        string chineseString = ""; // 初始化一个空字符串，用于存储提取的中文字符

        //将传入参数中的中文字符添加到结果字符串中
        for (int i = 0; i < str.Length; i++) // 遍历输入字符串的每个字符
        {
            if (str[i] >= 0x4E00 && str[i] <= 0x9FA5) // 判断字符是否在汉字的 Unicode 范围内
            {
                chineseString += str[i]; // 将汉字字符添加到结果字符串中
            }
        }

        //返回保留中文的处理结果
        return chineseString; // 返回包含所有提取的中文字符的字符串
    }

    public static double TryParseDouble(string text) // 定义一个静态方法，尝试将字符串转换为 double
    {
        _ = double.TryParse(text, out double result); // 尝试解析字符串为 double 类型，结果存储在 result 中
        return result; // 返回解析结果
    }

    public static int TryParseInt(string text) // 定义一个静态方法，尝试将字符串转换为 int
    {
        _ = int.TryParse(text, out int result); // 尝试解析字符串为 int 类型，结果存储在 result 中
        return result; // 返回解析结果
    }

    public static int TryExtractPositiveInt(string text) // 定义一个静态方法，提取并转换字符串中的正整数
    {
        if (string.IsNullOrEmpty(text)) // 如果输入的字符串为空或为 null
        {
            return -1; // 返回 -1 作为错误标志
        }

        try
        {
            text = RegexHelper.ExcludeNumberRegex().Replace(text, ""); // 使用正则表达式去除字符串中的所有非数字字符
            return int.Parse(text); // 尝试将剩余的字符串解析为 int 类型
        }
        catch
        {
            return -1; // 如果解析失败，返回 -1 作为错误标志
        }
    }

    [GeneratedRegex(@"[\u4e00-\u9fa5]")]
    private static partial Regex ChineseRegex(); // 定义一个私有的静态方法，返回匹配中文字符的正则表达式
}
```