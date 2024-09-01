# `.\better-genshin-impact\BetterGenshinImpact\Core\Config\AllConfig.cs`

```cs
﻿using BetterGenshinImpact.GameTask; // 引用 BetterGenshinImpact.GameTask 命名空间
using BetterGenshinImpact.GameTask.AutoCook; // 引用 BetterGenshinImpact.GameTask.AutoCook 命名空间
using BetterGenshinImpact.GameTask.AutoDomain; // 引用 BetterGenshinImpact.GameTask.AutoDomain 命名空间
using BetterGenshinImpact.GameTask.AutoFight; // 引用 BetterGenshinImpact.GameTask.AutoFight 命名空间
using BetterGenshinImpact.GameTask.AutoFishing; // 引用 BetterGenshinImpact.GameTask.AutoFishing 命名空间
using BetterGenshinImpact.GameTask.AutoGeniusInvokation; // 引用 BetterGenshinImpact.GameTask.AutoGeniusInvokation 命名空间
using BetterGenshinImpact.GameTask.AutoPick; // 引用 BetterGenshinImpact.GameTask.AutoPick 命名空间
using BetterGenshinImpact.GameTask.AutoSkip; // 引用 BetterGenshinImpact.GameTask.AutoSkip 命名空间
using BetterGenshinImpact.GameTask.AutoWood; // 引用 BetterGenshinImpact.GameTask.AutoWood 命名空间
using BetterGenshinImpact.GameTask.QuickTeleport; // 引用 BetterGenshinImpact.GameTask.QuickTeleport 命名空间
using BetterGenshinImpact.Service.Notification; // 引用 BetterGenshinImpact.Service.Notification 命名空间
using CommunityToolkit.Mvvm.ComponentModel; // 引用 CommunityToolkit.Mvvm.ComponentModel 命名空间
using Fischless.GameCapture; // 引用 Fischless.GameCapture 命名空间
using System; // 引用 System 命名空间
using System.ComponentModel; // 引用 System.ComponentModel 命名空间
using System.Text.Json.Serialization; // 引用 System.Text.Json.Serialization 命名空间

namespace BetterGenshinImpact.Core.Config; // 定义 BetterGenshinImpact.Core.Config 命名空间

/// <summary>
///     更好的原神配置
/// </summary>
[Serializable] // 使类可以被序列化
public partial class AllConfig : ObservableObject // 定义 AllConfig 类，继承自 ObservableObject
{
    /// <summary>
    ///     窗口捕获的方式
    /// </summary>
    [ObservableProperty] // 自动生成属性通知功能
    private string _captureMode = CaptureModes.BitBlt.ToString(); // 定义窗口捕获模式的字符串属性，初始值为 CaptureModes.BitBlt.ToString()

    /// <summary>
    ///     详细的错误日志
    /// </summary>
    [ObservableProperty] // 自动生成属性通知功能
    private bool _detailedErrorLogs; // 定义是否记录详细错误日志的布尔属性

    /// <summary>
    ///     不展示新版本提示的最新版本
    /// </summary>
    [ObservableProperty] // 自动生成属性通知功能
    private string _notShowNewVersionNoticeEndVersion = ""; // 定义不展示新版本提示的版本号，初始值为空字符串

    /// <summary>
    ///     触发器触发频率(ms)
    /// </summary>
    [ObservableProperty] // 自动生成属性通知功能
    private int _triggerInterval = 50; // 定义触发器触发的时间间隔，单位毫秒，初始值为 50

    /// <summary>
    ///     WGC使用位图缓存
    ///     高帧率情况下，可能会导致卡顿
    ///     云原神可能会出现黑屏
    /// </summary>
    [ObservableProperty] // 自动生成属性通知功能
    private bool _wgcUseBitmapCache = true; // 定义是否使用位图缓存，初始值为 true

    /// <summary>
    /// 推理使用的设备
    /// </summary>
    [ObservableProperty] // 自动生成属性通知功能
    private string _inferenceDevice = "CPU"; // 定义推理使用的设备类型，初始值为 "CPU"

    /// <summary>
    ///     遮罩窗口配置
    /// </summary>
    public MaskWindowConfig MaskWindowConfig { get; set; } = new(); // 定义遮罩窗口配置的属性，初始值为新的 MaskWindowConfig 对象

    /// <summary>
    ///     通用配置
    /// </summary>
    public CommonConfig CommonConfig { get; set; } = new(); // 定义通用配置的属性，初始值为新的 CommonConfig 对象

    /// <summary>
    ///     原神启动配置
    /// </summary>
    public GenshinStartConfig GenshinStartConfig { get; set; } = new(); // 定义原神启动配置的属性，初始值为新的 GenshinStartConfig 对象

    /// <summary>
    ///     自动拾取配置
    /// </summary>
    public AutoPickConfig AutoPickConfig { get; set; } = new(); // 定义自动拾取配置的属性，初始值为新的 AutoPickConfig 对象

    /// <summary>
    ///     自动剧情配置
    /// </summary>
    public AutoSkipConfig AutoSkipConfig { get; set; } = new(); // 定义自动剧情配置的属性，初始值为新的 AutoSkipConfig 对象

    /// <summary>
    ///     自动钓鱼配置
    /// </summary>
    public AutoFishingConfig AutoFishingConfig { get; set; } = new(); // 定义自动钓鱼配置的属性，初始值为新的 AutoFishingConfig 对象

    /// <summary>
    ///     快速传送配置
    /// </summary>
    public QuickTeleportConfig QuickTeleportConfig { get; set; } = new(); // 定义快速传送配置的属性，初始值为新的 QuickTeleportConfig 对象

    /// <summary>
    ///     自动烹饪配置
    /// </summary>
    public AutoCookConfig AutoCookConfig { get; set; } = new(); // 定义自动烹饪配置的属性，初始值为新的 AutoCookConfig 对象

    /// <summary>
    ///     自动打牌配置
    /// </summary>
    public AutoGeniusInvokationConfig AutoGeniusInvokationConfig { get; set; } = new(); // 定义自动打牌配置的属性，初始值为新的 AutoGeniusInvokationConfig 对象

    /// <summary>
    ///     自动伐木配置
    /// </summary>
    public AutoWoodConfig AutoWoodConfig { get; set; } = new(); // 定义自动伐木配置的属性，初始值为新的 AutoWoodConfig 对象

    /// <summary>
    ///     自动战斗配置
    /// </summary>
    # 自动战斗配置属性，初始化为新的 AutoFightConfig 实例
    public AutoFightConfig AutoFightConfig { get; set; } = new();

    /// <summary>
    ///     自动秘境配置
    /// </summary>
    # 自动秘境配置属性，初始化为新的 AutoDomainConfig 实例
    public AutoDomainConfig AutoDomainConfig { get; set; } = new();

    /// <summary>
    ///     脚本类配置
    /// </summary>
    # 脚本类配置属性，初始化为新的 MacroConfig 实例
    public MacroConfig MacroConfig { get; set; } = new();

    # 记录配置属性，初始化为新的 RecordConfig 实例
    public RecordConfig RecordConfig { get; set; } = new();

    /// <summary>
    ///     快捷键配置
    /// </summary>
    # 快捷键配置属性，初始化为新的 HotKeyConfig 实例
    public HotKeyConfig HotKeyConfig { get; set; } = new();

    /// <summary>
    ///     通知配置
    /// </summary>
    # 通知配置属性，初始化为新的 NotificationConfig 实例
    public NotificationConfig NotificationConfig { get; set; } = new();

    # 标记此属性不会被序列化为 JSON，初始化为 null 的可空 Action
    [JsonIgnore]
    public Action? OnAnyChangedAction { get; set; }

    # 初始化事件处理方法
    public void InitEvent()
    {
        # 将 OnAnyPropertyChanged 方法注册为 PropertyChanged 事件的处理程序
        PropertyChanged += OnAnyPropertyChanged;
        MaskWindowConfig.PropertyChanged += OnAnyPropertyChanged;
        CommonConfig.PropertyChanged += OnAnyPropertyChanged;
        GenshinStartConfig.PropertyChanged += OnAnyPropertyChanged;
        NotificationConfig.PropertyChanged += OnAnyPropertyChanged;
        # 将 OnNotificationPropertyChanged 方法注册为 NotificationConfig.PropertyChanged 事件的处理程序
        NotificationConfig.PropertyChanged += OnNotificationPropertyChanged;

        # 将 OnAnyPropertyChanged 方法注册为其他配置项的 PropertyChanged 事件的处理程序
        AutoPickConfig.PropertyChanged += OnAnyPropertyChanged;
        AutoSkipConfig.PropertyChanged += OnAnyPropertyChanged;
        AutoFishingConfig.PropertyChanged += OnAnyPropertyChanged;
        QuickTeleportConfig.PropertyChanged += OnAnyPropertyChanged;
        AutoCookConfig.PropertyChanged += OnAnyPropertyChanged;
        MacroConfig.PropertyChanged += OnAnyPropertyChanged;
        HotKeyConfig.PropertyChanged += OnAnyPropertyChanged;
        AutoWoodConfig.PropertyChanged += OnAnyPropertyChanged;
        AutoFightConfig.PropertyChanged += OnAnyPropertyChanged;
        AutoDomainConfig.PropertyChanged += OnAnyPropertyChanged;
    }

    # 处理任何配置项属性变化的方法
    public void OnAnyPropertyChanged(object? sender, EventArgs args)
    {
        # 刷新游戏任务管理器的触发器配置
        GameTaskManager.RefreshTriggerConfigs();
        # 如果 OnAnyChangedAction 不为空，调用它
        OnAnyChangedAction?.Invoke();
    }

    # 处理通知配置属性变化的方法
    public void OnNotificationPropertyChanged(object? sender, PropertyChangedEventArgs args)
    {
        # 刷新通知服务实例中的通知者
        NotificationService.Instance().RefreshNotifiers();
    }
# 结束当前代码块或语句（可能是在某个条件判断、循环、函数定义等的结束）
}
```