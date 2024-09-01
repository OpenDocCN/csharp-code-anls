# `.\better-genshin-impact\BetterGenshinImpact\Core\Script\Group\ScriptGroup.cs`

```cs
﻿using BetterGenshinImpact.Service;  // 引入 BetterGenshinImpact.Service 命名空间
using CommunityToolkit.Mvvm.ComponentModel;  // 引入 CommunityToolkit.Mvvm.ComponentModel 命名空间
using System;  // 引入 System 命名空间
using System.Collections.ObjectModel;  // 引入 System.Collections.ObjectModel 命名空间
using System.Collections.Specialized;  // 引入 System.Collections.Specialized 命名空间
using System.Text.Json;  // 引入 System.Text.Json 命名空间

namespace BetterGenshinImpact.Core.Script.Group;  // 定义命名空间 BetterGenshinImpact.Core.Script.Group

public partial class ScriptGroup : ObservableObject  // 定义 ScriptGroup 类，继承自 ObservableObject
{
    public int Index { get; set; }  // 公开的整型属性 Index

    [ObservableProperty]  // 指示此属性应生成通知变化的支持
    private string _name = string.Empty;  // 私有的字符串属性 _name，初始值为空字符串

    [ObservableProperty]  // 指示此属性应生成通知变化的支持
    private ObservableCollection<ScriptGroupProject> _projects = [];  // 私有的 ObservableCollection 属性 _projects，初始值为空集合

    public ScriptGroup()  // 构造函数
    {
        Projects.CollectionChanged += ProjectsCollectionChanged;  // 订阅 Projects 集合变化事件
    }

    private void ProjectsCollectionChanged(object? sender, NotifyCollectionChangedEventArgs e)  // 处理集合变化事件
    {
        OnPropertyChanged(nameof(Projects));  // 通知属性 Projects 已更改
    }

    public string ToJson()  // 将对象序列化为 JSON 字符串
    {
        return JsonSerializer.Serialize(this, ConfigService.JsonOptions);  // 使用 JsonSerializer 将当前对象序列化为 JSON 字符串
    }

    public static ScriptGroup FromJson(string json)  // 从 JSON 字符串反序列化为 ScriptGroup 对象
    {
        return JsonSerializer.Deserialize<ScriptGroup>(json, ConfigService.JsonOptions) ?? throw new Exception("解析配置组JSON配置失败");  // 使用 JsonSerializer 反序列化 JSON 字符串，若结果为 null，则抛出异常
    }
}
```