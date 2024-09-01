# `.\better-genshin-impact\BetterGenshinImpact\ViewModel\Pages\HotKeyPageViewModel.cs`

```cs
# 引入 BetterGenshinImpact 核心配置相关的命名空间
﻿using BetterGenshinImpact.Core.Config;
# 引入 BetterGenshinImpact 核心录制功能相关的命名空间
using BetterGenshinImpact.Core.Recorder;
# 引入 BetterGenshinImpact 核心脚本功能相关的命名空间
using BetterGenshinImpact.Core.Script;
# 引入 BetterGenshinImpact 游戏任务相关的命名空间
using BetterGenshinImpact.GameTask;
# 引入 BetterGenshinImpact 游戏任务自动战斗相关的命名空间
using BetterGenshinImpact.GameTask.AutoFight;
# 引入 BetterGenshinImpact 游戏任务自动追踪路径相关的命名空间
using BetterGenshinImpact.GameTask.AutoTrackPath;
# 引入 BetterGenshinImpact 游戏任务自动路径规划相关的命名空间
using BetterGenshinImpact.GameTask.AutoPathing;
# 引入 BetterGenshinImpact 游戏任务通用功能相关的命名空间
using BetterGenshinImpact.GameTask.Common;
# 引入 BetterGenshinImpact 游戏任务通用视觉功能相关的命名空间
using BetterGenshinImpact.GameTask.Common.BgiVision;
# 引入 BetterGenshinImpact 游戏任务宏功能相关的命名空间
using BetterGenshinImpact.GameTask.Macro;
# 引入 BetterGenshinImpact 游戏任务快速购买相关的命名空间
using BetterGenshinImpact.GameTask.QucikBuy;
# 引入 BetterGenshinImpact 游戏任务快速茶壶相关的命名空间
using BetterGenshinImpact.GameTask.QuickSereniteaPot;
# 引入 BetterGenshinImpact 辅助功能相关的命名空间
using BetterGenshinImpact.Helpers;
# 引入 BetterGenshinImpact 辅助扩展功能相关的命名空间
using BetterGenshinImpact.Helpers.Extensions;
# 引入 BetterGenshinImpact 模型相关的命名空间
using BetterGenshinImpact.Model;
# 引入 BetterGenshinImpact 服务接口相关的命名空间
using BetterGenshinImpact.Service.Interface;
# 引入 CommunityToolkit MVVM 组件模型相关的命名空间
using CommunityToolkit.Mvvm.ComponentModel;
# 引入 CommunityToolkit MVVM 消息传递相关的命名空间
using CommunityToolkit.Mvvm.Messaging;
# 引入 CommunityToolkit MVVM 消息传递消息类型的命名空间
using CommunityToolkit.Mvvm.Messaging.Messages;
# 引入 Microsoft 扩展日志记录相关的命名空间
using Microsoft.Extensions.Logging;
# 引入系统集合可观察集合相关的命名空间
using System.Collections.ObjectModel;
# 引入系统诊断相关的命名空间
using System.Diagnostics;
# 引入系统反射相关的命名空间
using System.Reflection;
# 引入系统线程相关的命名空间
using System.Threading;
# 引入 BetterGenshinImpact 游戏任务模型枚举相关的命名空间
using BetterGenshinImpact.GameTask.Model.Enum;
# 引入 Vanara PInvoke 库相关的命名空间
using Vanara.PInvoke;
# 从 Vanara PInvoke.User32 命名空间中引入静态成员
using static Vanara.PInvoke.User32;
# 为 HotKeySettingModel 类型起别名
using HotKeySettingModel = BetterGenshinImpact.Model.HotKeySettingModel;

# 定义命名空间 BetterGenshinImpact.ViewModel.Pages
namespace BetterGenshinImpact.ViewModel.Pages;

# 定义 HotKeyPageViewModel 类，部分公开，继承自 ObservableObject 和 IViewModel 接口
public partial class HotKeyPageViewModel : ObservableObject, IViewModel
{
    # 定义只读字段，记录日志
    private readonly ILogger<HotKeyPageViewModel> _logger;
    # 定义只读字段，存储任务设置页面视图模型
    private readonly TaskSettingsPageViewModel _taskSettingsPageViewModel;
    # 定义公开的属性，用于存储所有配置
    public AllConfig Config { get; set; }

    # 定义可观察属性，存储热键设置模型集合
    [ObservableProperty]
    private ObservableCollection<HotKeySettingModel> _hotKeySettingModels = [];

    # 定义构造函数，初始化视图模型
    public HotKeyPageViewModel(IConfigService configService, ILogger<HotKeyPageViewModel> logger, TaskSettingsPageViewModel taskSettingsPageViewModel)
    {
        _logger = logger; // 将传入的 logger 赋值给实例变量 _logger
        _taskSettingsPageViewModel = taskSettingsPageViewModel; // 将传入的 taskSettingsPageViewModel 赋值给实例变量 _taskSettingsPageViewModel
        // 获取配置
        Config = configService.Get(); // 从 configService 获取配置，并赋值给 Config

        // 构建快捷键配置列表
        BuildHotKeySettingModelList(); // 调用方法来构建快捷键设置模型列表

        foreach (var hotKeyConfig in HotKeySettingModels) // 遍历 HotKeySettingModels 列表中的每个快捷键配置模型
        {
            hotKeyConfig.RegisterHotKey(); // 注册当前快捷键配置模型的快捷键
            hotKeyConfig.PropertyChanged += (sender, e) => // 订阅快捷键配置模型的属性变化事件
            {
                if (sender is HotKeySettingModel model) // 如果发送者是 HotKeySettingModel 实例
                {
                    // 反射更新配置

                    // 更新快捷键
                    if (e.PropertyName == "HotKey") // 如果属性名是 "HotKey"
                    {
                        Debug.WriteLine($"{model.FunctionName} 快捷键变更为 {model.HotKey}"); // 打印快捷键变更信息
                        var pi = Config.HotKeyConfig.GetType().GetProperty(model.ConfigPropertyName, BindingFlags.Public | BindingFlags.Instance); // 通过反射获取配置属性
                        if (null != pi && pi.CanWrite) // 如果属性存在且可以写入
                        {
                            var str = model.HotKey.ToString(); // 获取快捷键的字符串表示
                            if (str == "< None >") // 如果快捷键表示为 "< None >"
                            {
                                str = ""; // 将其转换为空字符串
                            }

                            pi.SetValue(Config.HotKeyConfig, str, null); // 更新配置属性的值
                        }
                    }

                    // 更新快捷键类型
                    if (e.PropertyName == "HotKeyType") // 如果属性名是 "HotKeyType"
                    {
                        Debug.WriteLine($"{model.FunctionName} 快捷键类型变更为 {model.HotKeyType.ToChineseName()}"); // 打印快捷键类型变更信息
                        model.HotKey = HotKey.None; // 将当前模型的快捷键设置为 None
                        var pi = Config.HotKeyConfig.GetType().GetProperty(model.ConfigPropertyName + "Type", BindingFlags.Public | BindingFlags.Instance); // 通过反射获取快捷键类型配置属性
                        if (null != pi && pi.CanWrite) // 如果属性存在且可以写入
                        {
                            pi.SetValue(Config.HotKeyConfig, model.HotKeyType.ToString(), null); // 更新快捷键类型配置属性的值
                        }
                    }

                    RemoveDuplicateHotKey(model); // 移除重复的快捷键配置
                    model.UnRegisterHotKey(); // 注销当前模型的快捷键
                    model.RegisterHotKey(); // 重新注册当前模型的快捷键
                }
            };
        }
    }

    /// <summary>
    /// 移除重复的快捷键配置
    /// </summary>
    /// <param name="current"></param>
    private void RemoveDuplicateHotKey(HotKeySettingModel current) // 移除与当前模型快捷键相同的重复快捷键
    {
        if (current.HotKey.IsEmpty) // 如果当前快捷键为空
        {
            return; // 直接返回
        }

        foreach (var hotKeySettingModel in HotKeySettingModels) // 遍历 HotKeySettingModels 列表中的每个快捷键设置模型
        {
            if (hotKeySettingModel.HotKey.IsEmpty) // 如果遍历到的快捷键为空
            {
                continue; // 跳过该模型
            }

            if (hotKeySettingModel.ConfigPropertyName != current.ConfigPropertyName && hotKeySettingModel.HotKey == current.HotKey) // 如果配置属性名不同且快捷键相同
            {
                hotKeySettingModel.HotKey = HotKey.None; // 将该模型的快捷键设置为 None
            }
        }
    }

    private void BuildHotKeySettingModelList() // 声明构建快捷键设置模型列表的方法
    }

    private string ToChinese(bool enabled) // 声明将布尔值转换为中文的辅助方法
    {
        return enabled.ToChinese(); // 调用 ToChinese 扩展方法并返回结果
    }
请提供需要注释的具体代码内容，以便我可以为每行代码添加适当的注释。
```