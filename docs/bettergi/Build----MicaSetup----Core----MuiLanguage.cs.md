# `.\better-genshin-impact\Build\MicaSetup\Core\MuiLanguage.cs`

```cs
# 引入所需的命名空间
﻿using MicaSetup.Helper; 
using MicaSetup.Services; 
using System; 
using System.Collections.Generic; 
using System.Globalization; 
using System.IO; 
using System.Text.RegularExpressions; 
using System.Windows; 
using System.Windows.Baml2006; 
using System.Xaml; 

# 定义命名空间
namespace MicaSetup.Core; 

# 定义静态类 MuiLanguage
public static class MuiLanguage
{
    # <summary> 标签内提供了不同 Windows 版本的字体列表链接
    /// <summary>
    /// https://learn.microsoft.com/en-us/typography/fonts/windows_11_font_list
    /// https://learn.microsoft.com/en-us/typography/fonts/windows_10_font_list
    /// https://learn.microsoft.com/en-us/typography/fonts/windows_81_font_list
    /// https://learn.microsoft.com/en-us/typography/fonts/windows_8_font_list
    /// https://learn.microsoft.com/en-us/typography/fonts/windows_7_font_list
    /// </summary>
    # 定义静态属性 FontSelector，初始化为空的列表
    public static List<MuiLanguageFont> FontSelector { get; } = new(); 

    # 定义公共静态方法 SetupLanguage，用于设置语言
    public static void SetupLanguage()
    {
        # 调用 SetLanguage 方法，忽略其返回值
        _ = SetLanguage();
    }

    # 定义公共静态方法 SetLanguage，根据当前 UI 文化设置语言
    public static bool SetLanguage() => CultureInfo.CurrentUICulture.TwoLetterISOLanguageName switch
    {
        # 如果当前语言是中文，调用 SetLanguage("zh")
        "zh" => SetLanguage("zh"),
        # 如果当前语言是日文，调用 SetLanguage("ja")
        "ja" => SetLanguage("ja"),
        # 对于其他语言，或默认语言调用 SetLanguage("en")
        "en" or _ => SetLanguage("en"),
    };

    # 定义公共静态方法 SetLanguage，根据传入的语言名称设置语言
    public static bool SetLanguage(string name = "en")
    {
        # 如果当前应用程序为 null，返回 false
        if (Application.Current == null)
        {
            return false;
        }

        try
        {
            # 遍历当前应用程序的合并字典
            foreach (ResourceDictionary dictionary in Application.Current.Resources.MergedDictionaries)
            {
                # 如果字典的 Source 不为 null 并且匹配指定的 XAML 文件路径
                if (dictionary.Source != null && dictionary.Source.OriginalString.Equals($"/Resources/Languages/{name}.xaml", StringComparison.Ordinal))
                {
                    # 从合并字典中移除匹配的字典
                    Application.Current.Resources.MergedDictionaries.Remove(dictionary);
                    # 将匹配的字典重新添加到合并字典中
                    Application.Current.Resources.MergedDictionaries.Add(dictionary);
                    # 成功设置语言，返回 true
                    return true;
                }
            }
        }
        catch (Exception e)
        {
            # 捕获并忽略异常
            _ = e;
        }
        # 如果未成功设置语言，返回 false
        return false;
    }

    # 定义公共静态方法 Mui，通过指定的键获取本地化字符串
    public static string Mui(string key)
    {
        try
        {
            # 如果当前应用程序为 null，调用 MuiBaml 方法
            if (Application.Current == null)
            {
                return MuiBaml(key);
            }
            # 如果应用程序中的资源找到指定的键并且值为字符串，返回该字符串
            if (Application.Current!.FindResource(key) is string value)
            {
                return value;
            }
        }
        catch (Exception e)
        {
            # 捕获并忽略异常
            _ = e;
        }
        # 如果未找到本地化字符串，返回 null
        return null!;
    }

    # 定义公共静态方法 Mui，通过指定的键和参数格式化本地化字符串
    public static string Mui(string key, params object[] args)
    {
        # 获取本地化字符串并格式化后返回
        return string.Format(Mui(key)?.ToString(), args);
    }

    # 定义私有静态方法 MuiBaml，通过指定的键从 BAML 资源中获取本地化字符串
    private static string MuiBaml(string key)
    {
        try
        {
            # 获取资源 XAML 流
            using Stream resourceXaml = ResourceHelper.GetStream(new MuiLanguageService().GetXamlUriString());
            # 加载 BAML 文件并尝试从中读取指定的键值
            if (BamlHelper.LoadBaml(resourceXaml) is ResourceDictionary resourceDictionary)
            {
                return (resourceDictionary[key] as string)!;
            }
        }
        catch (Exception e)
        {
            # 捕获并忽略异常
            _ = e;
        }
        # 如果未找到本地化字符串，返回 null
        return null!;
    }
}

# 定义文件级静态类 BamlHelper
file static class BamlHelper
{
    // 从给定的流加载 BAML 数据
    public static object LoadBaml(Stream stream)
    {
        // 创建 Baml2006Reader 实例来读取 BAML 数据
        using Baml2006Reader reader = new(stream);
        // 创建 XamlObjectWriter 实例以将数据写入对象
        using XamlObjectWriter writer = new(reader.SchemaContext);

        // 遍历 BAML 数据的每一节点
        while (reader.Read())
        {
            // 将当前节点写入对象
            writer.WriteNode(reader);
        }
        // 返回结果对象
        return writer.Result;
    }
}

public class MuiLanguageFont
{
    // 用于表示字体的名称，允许为空
    public string? Name { get; set; }
    // 用于表示字体的第二名称，允许为空
    public string? TwoName { get; set; }
    // 用于表示字体的第三名称，允许为空
    public string? ThreeName { get; set; }

    // 用于表示资源字体文件名，允许为空
    public string? ResourceFontFileName { get; set; }
    // 用于表示资源字体系列名称，允许为空
    public string? ResourceFontFamilyName { get; set; }
    // 资源字体系列名称的组合，格式为 "./{ResourceFontFileName}#{ResourceFontFamilyName}"，如果任一部分为空则返回 null
    public string? ResourceFamilyName => !string.IsNullOrWhiteSpace(ResourceFontFileName) && !string.IsNullOrWhiteSpace(ResourceFontFamilyName) ? $"./{ResourceFontFileName}#{ResourceFontFamilyName}" : null!;

    // 系统字体系列名称，允许为空
    public string? SystemFamilyName { get; set; }
    // 系统字体备选系列名称，允许为空
    public string? SystemFamilyNameBackup { get; set; }
}

public static class MuiLanguageFontExtension
{
    // 设置 Name 属性，并清空其他名称属性，返回当前对象
    public static MuiLanguageFont OnNameOf(this MuiLanguageFont self, string name)
    {
        self.Name = name ?? throw new ArgumentNullException(nameof(name));
        self.TwoName = null!;
        self.ThreeName = null!;
        return self;
    }

    // 设置 TwoName 属性，并清空其他名称属性，返回当前对象
    public static MuiLanguageFont OnTwoNameOf(this MuiLanguageFont self, string twoName)
    {
        self.Name = null!;
        self.TwoName = twoName ?? throw new ArgumentNullException(nameof(twoName));
        self.ThreeName = null!;
        return self;
    }

    // 设置 ThreeName 属性，并清空其他名称属性，返回当前对象
    public static MuiLanguageFont OnThreeNameOf(this MuiLanguageFont self, string threeName)
    {
        self.Name = null!;
        self.TwoName = null!;
        self.ThreeName = threeName ?? throw new ArgumentNullException(nameof(threeName));
        return self;
    }

    // 清空所有名称属性，返回当前对象
    public static MuiLanguageFont OnAnyName(this MuiLanguageFont self)
    {
        self.Name = null!;
        self.TwoName = null!;
        self.ThreeName = null!;
        return self;
    }

    // 设置资源字体的文件名和系列名称，返回当前对象
    public static MuiLanguageFont ForResourceFont(this MuiLanguageFont self, string fontFileName, string familyName)
    {
        self.ResourceFontFileName = fontFileName ?? throw new ArgumentNullException(nameof(fontFileName));
        self.ResourceFontFamilyName = familyName ?? throw new ArgumentNullException(nameof(familyName));
        return self;
    }

    // 设置系统字体系列名称及备选名称，返回当前对象，备选名称默认为空
    public static MuiLanguageFont ForSystemFont(this MuiLanguageFont self, string familyName, string familyNameBackup = null!)
    {
        self.SystemFamilyName = familyName ?? throw new ArgumentNullException(nameof(familyName));
        self.SystemFamilyNameBackup = familyNameBackup;
        // 验证系统字体系列名称是否符合正则表达式格式，否则抛出 ArgumentException 异常
        _ = !new Regex("^[a-zA-Z ]+$").IsMatch(familyName) ? throw new ArgumentException(nameof(familyName)) : default(object);
        return self;
    }
}
```