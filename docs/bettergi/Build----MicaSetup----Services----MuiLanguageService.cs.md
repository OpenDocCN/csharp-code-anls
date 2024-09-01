# `.\better-genshin-impact\Build\MicaSetup\Services\MuiLanguageService.cs`

```cs
# 使用 MicaSetup.Helper 命名空间的功能
using MicaSetup.Helper;
# 引入系统相关的类和函数
using System;
using System.Diagnostics;
using System.Globalization;
using System.Linq;
# 引入用于 WPF 的媒体功能
using System.Windows.Media;

# 定义 MicaSetup.Services 命名空间
namespace MicaSetup.Services;

# 禁用 IDE0002 警告，表示该代码行不符合某些 IDE 的代码规范
#pragma warning disable IDE0002

# 定义 MuiLanguageService 类，继承自 IMuiLanguageService 接口
public class MuiLanguageService : IMuiLanguageService
{
    # 定义 FontFamily 属性，用于设置或获取字体家族，初始化时设置为 null!
    public FontFamily FontFamily { get; set; } = null!;

    # 静态构造函数，类加载时调用，用于调试输出
    static MuiLanguageService()
    {
        # 调用调试方法输出信息
        DebugPrintPrivate();
    }

    # 定义 GetFontFamily 方法，获取当前的字体家族
    public FontFamily GetFontFamily()
    {
        # 如果 FontFamily 属性为 null，则进行以下处理
        if (FontFamily == null)
        {
            # 定义一个静态方法 GetUriString，根据字体文件名称生成 URI 字符串
            static string GetUriString(string name = null!) => $"pack://application:,,,/MicaSetup;component/Resources/Fonts/{name ?? string.Empty}";

            # 如果 FontSelector 列表中包含字体项
            if (FontSelector.Count > 0)
            {
                # 根据当前 UI 文化的信息查找匹配的字体项
                MuiLanguageFont font = MuiLanguage.FontSelector.Where(f => f.Name == CultureInfo.CurrentUICulture.Name).ToList().FirstOrDefault()
                    # 如果没有找到匹配的字体项，则根据 ThreeLetterISOLanguageName 查找
                    ?? MuiLanguage.FontSelector.Where(f => f.ThreeName == CultureInfo.CurrentUICulture.ThreeLetterISOLanguageName).ToList().FirstOrDefault()
                    # 如果仍然没有找到，则根据 TwoLetterISOLanguageName 查找
                    ?? MuiLanguage.FontSelector.Where(f => f.TwoName == CultureInfo.CurrentUICulture.TwoLetterISOLanguageName).ToList().FirstOrDefault()
                    # 如果都没有找到，则选择没有特定名称的字体项
                    ?? MuiLanguage.FontSelector.Where(f => f.Name == null && f.TwoName == null && f.ThreeName == null).ToList().FirstOrDefault();

                # 如果找到匹配的字体项
                if (font != null)
                {
                    # 如果有资源字体家族名称
                    if (!string.IsNullOrWhiteSpace(font.ResourceFamilyName))
                    {
                        # 如果资源文件存在，则创建 FontFamily 对象
                        if (ResourceHelper.HasResource(GetUriString(font.ResourceFontFileName!)))
                        {
                            FontFamily = new FontFamily(new Uri(GetUriString()), font.ResourceFamilyName);
                        }
                    }
                    # 如果没有资源字体家族名称，则检查系统字体名称
                    else if (!string.IsNullOrWhiteSpace(font.SystemFamilyName))
                    {
                        # 如果系统字体存在，则创建 FontFamily 对象
                        if (SystemFontHelper.HasFontFamily(font.SystemFamilyName!))
                        {
                            FontFamily = new FontFamily(font.SystemFamilyName);
                        }
                        # 如果系统字体不存在，则检查备用字体名称
                        else if (SystemFontHelper.HasFontFamily(font.SystemFamilyNameBackup!))
                        {
                            FontFamily = new FontFamily(font.SystemFamilyNameBackup);
                        }
                    }
                }
            }
            # 如果 FontSelector 列表为空
            else
            {
                # 如果当前 UI 文化是日语，使用特定的字体
                if (CultureInfo.CurrentUICulture.TwoLetterISOLanguageName == "ja")
                {
                    FontFamily = new FontFamily("Yu Gothic UI");
                }
                # 如果不是日语，则检查资源文件是否存在
                else
                {
                    if (ResourceHelper.HasResource(GetUriString("HarmonyOS_Sans_SC_Regular.ttf")))
                    {
                        FontFamily = new FontFamily(new Uri(GetUriString()), "./HarmonyOS_Sans_SC_Regular.ttf#HarmonyOS Sans SC");
                    }
                }
            }
        }
        # 返回当前的 FontFamily 属性值，如果为空则创建一个新的 FontFamily 对象
        return FontFamily ??= new FontFamily();
    }

    # 定义 GetXamlUriString 方法
    public string GetXamlUriString()
    # 静态方法，生成指定语言资源的 URI 字符串
    static string GetUriString(string name) => $"pack://application:,,,/MicaSetup;component/Resources/Languages/{name}.xaml";

    # 检查当前文化名称对应的资源是否存在
    if (ResourceHelper.HasResource(GetUriString(CultureInfo.CurrentUICulture.Name)))
    {
        # 如果存在，返回该 URI 字符串
        return GetUriString(CultureInfo.CurrentUICulture.Name);
    }
    else
    {
        # 如果不存在，检查当前文化的两字母语言名称对应的资源是否存在
        if (ResourceHelper.HasResource(GetUriString(CultureInfo.CurrentUICulture.TwoLetterISOLanguageName)))
        {
            # 如果存在，返回该 URI 字符串
            return GetUriString(CultureInfo.CurrentUICulture.TwoLetterISOLanguageName);
        }
        else
        {
            # 如果不存在，检查当前文化的三字母语言名称对应的资源是否存在
            if (ResourceHelper.HasResource(GetUriString(CultureInfo.CurrentUICulture.ThreeLetterISOLanguageName)))
            {
                # 如果存在，返回该 URI 字符串
                return GetUriString(CultureInfo.CurrentUICulture.ThreeLetterISOLanguageName);
            }
        }
    }

    # 如果以上所有检查都未找到资源，记录调试日志并返回默认的英文资源 URI
    Logger.Debug($"[MuiLanguageService] NotFound with match mui lang name of '{CultureInfo.CurrentUICulture.Name}' or '{CultureInfo.CurrentUICulture.TwoLetterISOLanguageName}' or '{CultureInfo.CurrentUICulture.ThreeLetterISOLanguageName}'.");
    return GetUriString("en");

    # 公共方法，获取许可证的 URI 字符串
    public string GetLicenseUriString()
    {
        # 静态方法，生成指定许可证语言资源的 URI 字符串
        static string GetUriString(string name) => $"pack://application:,,,/MicaSetup;component/Resources/Licenses/license.{name}.txt";

        # 检查当前文化名称对应的许可证资源是否存在
        if (ResourceHelper.HasResource(GetUriString(CultureInfo.CurrentUICulture.Name)))
        {
            # 如果存在，返回该 URI 字符串
            return GetUriString(CultureInfo.CurrentUICulture.Name);
        }
        else
        {
            # 如果不存在，检查当前文化的两字母语言名称对应的许可证资源是否存在
            if (ResourceHelper.HasResource(GetUriString(CultureInfo.CurrentUICulture.TwoLetterISOLanguageName)))
            {
                # 如果存在，返回该 URI 字符串
                return GetUriString(CultureInfo.CurrentUICulture.TwoLetterISOLanguageName);
            }
            else
            {
                # 如果不存在，检查当前文化的三字母语言名称对应的许可证资源是否存在
                if (ResourceHelper.HasResource(GetUriString(CultureInfo.CurrentUICulture.ThreeLetterISOLanguageName)))
                {
                    # 如果存在，返回该 URI 字符串
                    return GetUriString(CultureInfo.CurrentUICulture.ThreeLetterISOLanguageName);
                }
            }
        }
        # 如果以上所有检查都未找到许可证资源，记录调试日志并返回默认的英文许可证 URI
        Logger.Debug($"[MuiLanguageService] NotFound with match mui license name of '{CultureInfo.CurrentUICulture.Name}' or '{CultureInfo.CurrentUICulture.TwoLetterISOLanguageName}' or '{CultureInfo.CurrentUICulture.ThreeLetterISOLanguageName}'.");
        return GetUriString("en");
    }

    # 定义一个仅在调试模式下编译的方法
    [Conditional("DEBUG")]
    private static void DebugPrintPrivate()
    # 获取所有区域性文化信息
    CultureInfo[] cultures = CultureInfo.GetCultures(CultureTypes.AllCultures);
    # 记录调试信息，包括显示名称、名称、两字母语言名、三字母语言名和 IETF 标签
    Logger.Debug("Display Name", "Name", "TwoName", "ThreeName", "IetfTag");

    # 遍历每一个文化信息
    foreach (var culture in cultures)
    {
        # 如果文化类型是用户自定义文化，则跳过
        if ((culture.CultureTypes & CultureTypes.UserCustomCulture) == CultureTypes.UserCustomCulture)
        {
            continue;
        }
        # 记录当前文化信息的详细调试信息
        Logger.Debug(culture.DisplayName, culture.Name, culture.TwoLetterISOLanguageName, culture.ThreeLetterISOLanguageName, culture.IetfLanguageTag);
        # 如果显示名称包含特定标记，则触发调试中断
        if (culture.DisplayName.Contains("__EDITME__"))
        {
            Debugger.Break();
        }
    }
你提供的代码似乎不完整或缺失了上下文。可以提供更多信息或完整的代码吗？
```