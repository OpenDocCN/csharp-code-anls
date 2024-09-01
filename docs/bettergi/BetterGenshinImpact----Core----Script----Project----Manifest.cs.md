# `.\better-genshin-impact\BetterGenshinImpact\Core\Script\Project\Manifest.cs`

```cs
# 使用 BetterGenshinImpact.Service 命名空间
using BetterGenshinImpact.Service;
# 使用系统基础命名空间
using System;
using System.Collections.Generic;
using System.IO;
using System.Text.Json;
# 使用 BetterGenshinImpact.Core.Config 和 BetterGenshinImpact.Model 命名空间
using BetterGenshinImpact.Core.Config;
using BetterGenshinImpact.Model;

# 定义 Manifest 类，并将其标记为可序列化
namespace BetterGenshinImpact.Core.Script.Project;

[Serializable]
public class Manifest
{
    # Manifest 的版本号，默认为 1
    public int ManifestVersion { get; set; } = 1;
    # Manifest 的名称，默认为空字符串
    public string Name { get; set; } = string.Empty;
    # Manifest 的版本号，默认为空字符串
    public string Version { get; set; } = string.Empty;
    # Manifest 的描述，默认为空字符串
    public string Description { get; set; } = string.Empty;
    # 作者列表，默认为空列表
    public List<Author> Authors { get; set; } = [];
    # 主脚本文件名，默认为空字符串
    public string Main { get; set; } = string.Empty;
    # 设置 UI 文件名，默认为空字符串
    public string SettingsUi { get; set; } = string.Empty;
    # 脚本文件列表，默认为空数组
    public string[] Scripts { get; set; } = [];
    # 库文件列表，默认为空数组
    public string[] Library { get; set; } = [];

    # 从 JSON 字符串创建 Manifest 实例
    public static Manifest FromJson(string json)
    {
        # 尝试将 JSON 字符串 deserialized 成 Manifest 对象，如果失败则抛出异常
        var manifest = JsonSerializer.Deserialize<Manifest>(json, Global.ManifestJsonOptions) ?? throw new Exception("Failed to deserialize JSON.");
        return manifest;
    }

    # 验证 Manifest 对象的有效性
    public void Validate(string path)
    {
        # 检查 Name 是否为空或只包含空白字符，如果是则抛出异常
        if (string.IsNullOrWhiteSpace(Name))
        {
            throw new Exception("manifest.json: name is required.");
        }

        # 检查 Version 是否为空或只包含空白字符，如果是则抛出异常
        if (string.IsNullOrWhiteSpace(Version))
        {
            throw new Exception("manifest.json: version is required.");
        }

        # 检查 Main 是否为空或只包含空白字符，如果是则抛出异常
        if (string.IsNullOrWhiteSpace(Main))
        {
            throw new Exception("manifest.json: main script is required.");
        }

        # 检查指定路径下的 Main 文件是否存在，如果不存在则抛出文件未找到异常
        if (!File.Exists(Path.Combine(path, Main)))
        {
            throw new FileNotFoundException("main js file not found.");
        }
    }

    # 从指定路径加载设置项
    public List<SettingItem> LoadSettingItems(string path)
    {
        # 如果 SettingsUi 为空或只包含空白字符，则返回空列表
        if (string.IsNullOrWhiteSpace(SettingsUi))
        {
            return [];
        }

        # 初始化设置项列表
        var settingItems = new List<SettingItem>();
        # 构建设置文件的完整路径
        var settingFile = Path.Combine(path, SettingsUi);
        # 如果设置文件存在，则读取并反序列化为设置项列表
        if (File.Exists(settingFile))
        {
            var json = File.ReadAllText(settingFile);
            settingItems = JsonSerializer.Deserialize<List<SettingItem>>(json, ConfigService.JsonOptions) ?? [];
        }
        # 返回设置项列表
        return settingItems;
    }
}
```