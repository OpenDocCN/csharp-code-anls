# `.\better-genshin-impact\BetterGenshinImpact\Core\Config\Global.cs`

```cs
# 引入 BetterGenshinImpact.Service 命名空间中的类和方法
using BetterGenshinImpact.Service;
# 引入系统基本命名空间
using System;
# 引入文件和流操作相关命名空间
using System.IO;
# 引入 JSON 序列化和反序列化相关命名空间
using System.Text.Json.Serialization;
# 引入 JSON 操作相关命名空间
using System.Text.Json;

# 定义 BetterGenshinImpact.Core.Config 命名空间
namespace BetterGenshinImpact.Core.Config;

# 定义 Global 类
public class Global
{
    # 定义静态只读属性 Version，表示当前版本号
    public static string Version { get; } = "0.33.3";

    # 定义静态属性 StartUpPath，表示应用程序启动路径，默认为 AppContext.BaseDirectory
    public static string StartUpPath { get; set; } = AppContext.BaseDirectory;

    # 定义静态只读属性 ManifestJsonOptions，表示 JSON 序列化选项
    public static readonly JsonSerializerOptions ManifestJsonOptions = new()
    {
        # 允许使用命名浮点数文字
        NumberHandling = JsonNumberHandling.AllowNamedFloatingPointLiterals,
        # 使用不安全的 JSON 转义以放宽限制
        Encoder = System.Text.Encodings.Web.JavaScriptEncoder.UnsafeRelaxedJsonEscaping,
        # 使用蛇形命名法（蛇形小写）作为属性命名策略
        PropertyNamingPolicy = JsonNamingPolicy.SnakeCaseLower,
        # JSON 输出格式化为缩进形式
        WriteIndented = true,
        # 允许 JSON 末尾存在逗号
        AllowTrailingCommas = true,
        # 跳过 JSON 注释的处理
        ReadCommentHandling = JsonCommentHandling.Skip,
    };

    # 定义静态方法 Absolute，将相对路径转换为绝对路径
    public static string Absolute(string relativePath)
    {
        return Path.Combine(StartUpPath, relativePath);
    }

    # 定义静态方法 ScriptPath，返回 "Script" 文件夹的绝对路径
    public static string ScriptPath()
    {
        return Absolute("Script");
    }

    # 定义重载的静态方法 ScriptPath，接受文件夹名称并返回包含该文件夹的路径
    public static string ScriptPath(string folderName)
    {
        return Path.Combine(Absolute("Script"), folderName);
    }

    # 定义静态方法 ReadAllTextIfExist，读取相对路径文件的内容，如果文件存在
    public static string? ReadAllTextIfExist(string relativePath)
    {
        # 获取绝对路径
        var path = Absolute(relativePath);
        # 如果文件存在，读取文件内容并返回
        if (File.Exists(path)) return File.ReadAllText(path);
        # 文件不存在，返回 null
        return null;
    }

    # 定义静态方法 IsNewVersion，比较当前版本和给定版本，判断是否为新版本
    /// <summary>
    ///     新获取到的版本号与当前版本号比较，判断是否为新版本
    /// </summary>
    /// <param name="currentVersion">新获取到的版本</param>
    /// <returns></returns>
    public static bool IsNewVersion(string currentVersion)
    {
        return IsNewVersion(Version, currentVersion);
    }

    # 定义静态方法 IsNewVersion，比较两个版本号，判断是否需要更新
    /// <summary>
    ///     新获取到的版本号与当前版本号比较，判断是否为新版本
    /// </summary>
    /// <param name="oldVersion">老版本</param>
    /// <param name="currentVersion">新获取到的版本</param>
    /// <returns>是否需要更新</returns>
    public static bool IsNewVersion(string oldVersion, string currentVersion)
    {
        try
        {
            # 尝试将版本号字符串转换为 Version 对象
            Version oldVersionX = new(oldVersion);
            Version currentVersionX = new(currentVersion);

            # 如果新版本大于旧版本，返回 true 表示需要更新
            if (currentVersionX > oldVersionX)
                return true;
        }
        catch
        {
            # 捕获任何异常并忽略
            ///
        }

        # 如果没有满足更新条件，返回 false
        return false;
    }

    # 定义静态方法 WriteAllText，将字符串内容写入指定路径的文件
    public static void WriteAllText(string relativePath, string blackListJson)
    {
        # 获取绝对路径
        var path = Absolute(relativePath);
        # 将字符串内容写入文件
        File.WriteAllText(path, blackListJson);
    }
}
```