# `.\better-genshin-impact\BetterGenshinImpact\Core\Script\Group\ScriptGroupProject.cs`

```cs
﻿using BetterGenshinImpact.Core.Config;  // 引入配置相关的命名空间
using BetterGenshinImpact.Core.Recorder;  // 引入录制相关的命名空间
using BetterGenshinImpact.Core.Script.Project;  // 引入脚本项目相关的命名空间
using CommunityToolkit.Mvvm.ComponentModel;  // 引入 MVVM 组件模型命名空间
using System;  // 引入基础系统功能命名空间
using System.Collections.Generic;  // 引入集合相关命名空间
using System.Dynamic;  // 引入动态对象支持命名空间
using System.IO;  // 引入文件输入输出操作命名空间
using System.Text.Json.Serialization;  // 引入 JSON 序列化支持命名空间
using System.Threading.Tasks;  // 引入异步任务操作命名空间

namespace BetterGenshinImpact.Core.Script.Group;  // 定义命名空间

public partial class ScriptGroupProject : ObservableObject  // 定义一个公开的部分类，继承自 ObservableObject（支持属性变化通知）
{
    [ObservableProperty]  // 自动生成属性通知支持代码的特性
    private int _index;  // 定义私有整型字段，用于存储索引

    public string Name { get; set; } = string.Empty;  // 定义公开的字符串属性，用于存储名称，默认为空字符串

    public string FolderName { get; set; } = string.Empty;  // 定义公开的字符串属性，用于存储文件夹名称，默认为空字符串

    [ObservableProperty]  // 自动生成属性通知支持代码的特性
    private string _type = string.Empty;  // 定义私有字符串字段，用于存储类型，默认为空字符串

    [JsonIgnore]  // 忽略 JSON 序列化/反序列化时此属性
    public string TypeDesc => ScriptGroupProjectExtensions.TypeDescriptions[Type];  // 获取类型描述的属性，通过类型从扩展方法获取描述

    [ObservableProperty]  // 自动生成属性通知支持代码的特性
    private string _status = string.Empty;  // 定义私有字符串字段，用于存储状态，默认为空字符串

    [JsonIgnore]  // 忽略 JSON 序列化/反序列化时此属性
    public string StatusDesc => ScriptGroupProjectExtensions.StatusDescriptions[Status];  // 获取状态描述的属性，通过状态从扩展方法获取描述

    /// <summary>
    /// 执行周期
    /// 不在 ScheduleDescriptions 中则会被视为自定义Cron表达式
    /// </summary>
    [ObservableProperty]  // 自动生成属性通知支持代码的特性
    private string _schedule = string.Empty;  // 定义私有字符串字段，用于存储执行周期，默认为空字符串

    [JsonIgnore]  // 忽略 JSON 序列化/反序列化时此属性
    public string ScheduleDesc => ScriptGroupProjectExtensions.ScheduleDescriptions.GetValueOrDefault(Schedule, "自定义周期");  // 获取周期描述的属性，通过周期从扩展方法获取描述，若无匹配则返回“自定义周期”

    [ObservableProperty]  // 自动生成属性通知支持代码的特性
    private int _runNum = 1;  // 定义私有整型字段，用于存储运行次数，默认为1

    [JsonIgnore]  // 忽略 JSON 序列化/反序列化时此属性
    public ScriptProject? Project { get; set; }  // 定义公开的可空类型 ScriptProject 属性，用于存储脚本项目

    public ExpandoObject? JsScriptSettingsObject { get; set; }  // 定义公开的可空类型 ExpandoObject 属性，用于存储 JavaScript 脚本设置对象

    public ScriptGroupProject()  // 默认构造函数
    {
    }

    public ScriptGroupProject(ScriptProject project)  // 带参数的构造函数，根据传入的 ScriptProject 对象初始化属性
    {
        Name = project.Manifest.Name;  // 初始化 Name 属性为项目的名称
        FolderName = project.FolderName;  // 初始化 FolderName 属性为项目的文件夹名称
        Status = "Enabled";  // 设置默认状态为“启用”
        Schedule = "Daily";  // 设置默认执行周期为“每日”
        Project = project;  // 设置项目属性为传入的项目对象
        Type = "Javascript";  // 设置类型为“JavaScript”
    }

    public ScriptGroupProject(string kmName)  // 带参数的构造函数，根据传入的名称初始化属性
    {
        Name = kmName;  // 初始化 Name 属性为传入的名称
        FolderName = kmName;  // 初始化 FolderName 属性为传入的名称
        Status = "Enabled";  // 设置默认状态为“启用”
        Schedule = "Daily";  // 设置默认执行周期为“每日”
        Project = null;  // 设置项目属性为 null（表示非 JavaScript 脚本）
        Type = "KeyMouse";  // 设置类型为“KeyMouse”
    }

    /// <summary>
    /// 通过 FolderName 查找 ScriptProject
    /// </summary>
    public void BuildScriptProjectRelation()  // 建立脚本项目关系的方法
    {
        if (string.IsNullOrEmpty(FolderName))  // 检查 FolderName 是否为空或 null
        {
            throw new Exception("FolderName 为空");  // 抛出异常，提示 FolderName 为空
        }
        Project = new ScriptProject(FolderName);  // 根据 FolderName 创建新的 ScriptProject 对象并赋值给 Project 属性
    }

    public async Task Run()  // 异步执行脚本的方法
    {
        if (Type == "Javascript")  // 如果类型为“JavaScript”
        {
            if (Project == null)  // 检查 Project 是否为 null
            {
                throw new Exception("JS脚本未初始化");  // 抛出异常，提示 JS 脚本未初始化
            }
            await Project.ExecuteAsync(JsScriptSettingsObject);  // 异步执行项目中的脚本
        }
        if (Type == "KeyMouse")  // 如果类型为“KeyMouse”
        {
            // 加载并执行
            var json = await File.ReadAllTextAsync(Global.Absolute(@$"User\KeyMouseScript\{Name}"));  // 异步读取 KeyMouse 脚本文件内容
            await KeyMouseMacroPlayer.PlayMacro(json, CancellationContext.Instance.Cts.Token, false);  // 异步执行读取到的 KeyMouse 宏脚本
        }
        else  // 如果类型不匹配
        {
            //throw new Exception("不支持的脚本类型");  // 抛出异常，提示不支持的脚本类型（此行被注释掉了）
        }
    }

    partial void OnTypeChanged(string value)  // 部分方法，用于处理 Type 属性更改事件
    {
        OnPropertyChanged(nameof(TypeDesc));  // 触发 TypeDesc 属性变化通知
    }
    # 部分方法，当状态改变时调用
    partial void OnStatusChanged(string value)
    {
        # 通知属性变化，更新状态描述
        OnPropertyChanged(nameof(StatusDesc));
    }

    # 部分方法，当计划改变时调用
    partial void OnScheduleChanged(string value)
    {
        # 通知属性变化，更新计划描述
        OnPropertyChanged(nameof(ScheduleDesc));
    }
}
# 类定义的结束符号

public class ScriptGroupProjectExtensions
{
    # 定义一个只读的静态字典，用于存储类型描述
    public static readonly Dictionary<string, string> TypeDescriptions = new()
    {
        # 键为 "Javascript"，值为 "JS脚本"
        { "Javascript", "JS脚本" },
        # 键为 "KeyMouse"，值为 "键鼠脚本"
        { "KeyMouse", "键鼠脚本" }
    };

    # 定义一个只读的静态字典，用于存储状态描述
    public static readonly Dictionary<string, string> StatusDescriptions = new()
    {
        # 键为 "Enabled"，值为 "启用"
        { "Enabled", "启用" },
        # 键为 "Disabled"，值为 "禁用"
        { "Disabled", "禁用" }
    };

    # 返回 StatusDescriptions 字典的方法
    public Dictionary<string, string> GetStatusDescriptions()
    {
        return StatusDescriptions;
    }

    # 定义一个只读的静态字典，用于存储计划描述
    public static readonly Dictionary<string, string> ScheduleDescriptions = new()
    {
        # 键为 "Daily"，值为 "每日"
        { "Daily", "每日" },
        # 键为 "EveryTwoDays"，值为 "间隔两天"
        { "EveryTwoDays", "间隔两天" },
        # 键为 "Monday"，值为 "每周一"
        { "Monday", "每周一" },
        # 键为 "Tuesday"，值为 "每周二"
        { "Tuesday", "每周二" },
        # 键为 "Wednesday"，值为 "每周三"
        { "Wednesday", "每周三" },
        # 键为 "Thursday"，值为 "每周四"
        { "Thursday", "每周四" },
        # 键为 "Friday"，值为 "每周五"
        { "Friday", "每周五" },
        # 键为 "Saturday"，值为 "每周六"
        { "Saturday", "每周六" },
        # 键为 "Sunday"，值为 "每周日"
        { "Sunday", "每周日" }
    };

    # 返回 ScheduleDescriptions 字典的方法
    public Dictionary<string, string> GetScheduleDescriptions()
    {
        return ScheduleDescriptions;
    }
}
```