# `.\better-genshin-impact\BetterGenshinImpact\Core\Config\HotKeyConfig.cs`

```cs
﻿using BetterGenshinImpact.Model; // 引用 BetterGenshinImpact.Model 命名空间
using CommunityToolkit.Mvvm.ComponentModel; // 引用 CommunityToolkit.Mvvm.ComponentModel 命名空间
using System; // 引用 System 命名空间

namespace BetterGenshinImpact.Core.Config; // 定义 BetterGenshinImpact.Core.Config 命名空间

/// <summary>
///     格式必须是 快捷键 与 快捷键Type
/// </summary>
[Serializable] // 使类可序列化
public partial class HotKeyConfig : ObservableObject // 定义 HotKeyConfig 类，继承 ObservableObject 类
{
    [ObservableProperty] // 标记字段为可观察属性
    private string _autoTrackHotkey = ""; // 自动跟踪快捷键

    [ObservableProperty] // 标记字段为可观察属性
    private string _autoTrackHotkeyType = HotKeyTypeEnum.KeyboardMonitor.ToString(); // 自动跟踪快捷键类型，默认为 KeyboardMonitor

    [ObservableProperty] // 标记字段为可观察属性
    private string _autoDomainHotkey = ""; // 自动领域快捷键

    [ObservableProperty] // 标记字段为可观察属性
    private string _autoDomainHotkeyType = HotKeyTypeEnum.KeyboardMonitor.ToString(); // 自动领域快捷键类型，默认为 KeyboardMonitor

    [ObservableProperty] // 标记字段为可观察属性
    private string _autoFightHotkey = ""; // 自动战斗快捷键

    [ObservableProperty] // 标记字段为可观察属性
    private string _autoFightHotkeyType = HotKeyTypeEnum.KeyboardMonitor.ToString(); // 自动战斗快捷键类型，默认为 KeyboardMonitor

    [ObservableProperty] // 标记字段为可观察属性
    private string _autoFishingEnabledHotkey = ""; // 自动钓鱼启用快捷键

    [ObservableProperty] // 标记字段为可观察属性
    private string _autoFishingEnabledHotkeyType = HotKeyTypeEnum.KeyboardMonitor.ToString(); // 自动钓鱼启用快捷键类型，默认为 KeyboardMonitor

    [ObservableProperty] // 标记字段为可观察属性
    private string _autoGeniusInvokationHotkey = ""; // 自动天才召唤快捷键

    [ObservableProperty] // 标记字段为可观察属性
    private string _autoGeniusInvokationHotkeyType = HotKeyTypeEnum.KeyboardMonitor.ToString(); // 自动天才召唤快捷键类型，默认为 KeyboardMonitor

    [ObservableProperty] // 标记字段为可观察属性
    private string _autoPickEnabledHotkey = ""; // 自动拾取启用快捷键

    [ObservableProperty] // 标记字段为可观察属性
    private string _autoPickEnabledHotkeyType = HotKeyTypeEnum.KeyboardMonitor.ToString(); // 自动拾取启用快捷键类型，默认为 KeyboardMonitor

    [ObservableProperty] // 标记字段为可观察属性
    private string _autoSkipEnabledHotkey = ""; // 自动跳过启用快捷键

    [ObservableProperty] // 标记字段为可观察属性
    private string _autoSkipEnabledHotkeyType = HotKeyTypeEnum.KeyboardMonitor.ToString(); // 自动跳过启用快捷键类型，默认为 KeyboardMonitor

    [ObservableProperty] // 标记字段为可观察属性
    private string _autoSkipHangoutEnabledHotkey = ""; // 自动跳过外出启用快捷键

    [ObservableProperty] // 标记字段为可观察属性
    private string _autoSkipHangoutEnabledHotkeyType = HotKeyTypeEnum.KeyboardMonitor.ToString(); // 自动跳过外出启用快捷键类型，默认为 KeyboardMonitor

    [ObservableProperty] // 标记字段为可观察属性
    private string _autoWoodHotkey = ""; // 自动木材快捷键

    [ObservableProperty] // 标记字段为可观察属性
    private string _autoWoodHotkeyType = HotKeyTypeEnum.KeyboardMonitor.ToString(); // 自动木材快捷键类型，默认为 KeyboardMonitor

    [ObservableProperty] // 标记字段为可观察属性
    private string _bgiEnabledHotkey = "F11"; // BGI 启用快捷键，默认值为 "F11"

    [ObservableProperty] // 标记字段为可观察属性
    private string _bgiEnabledHotkeyType = HotKeyTypeEnum.GlobalRegister.ToString(); // BGI 启用快捷键类型，默认为 GlobalRegister

    [ObservableProperty] // 标记字段为可观察属性
    private string _enhanceArtifactHotkey = ""; // 提升艺术品快捷键

    [ObservableProperty] // 标记字段为可观察属性
    private string _enhanceArtifactHotkeyType = HotKeyTypeEnum.KeyboardMonitor.ToString(); // 提升艺术品快捷键类型，默认为 KeyboardMonitor

    [ObservableProperty] // 标记字段为可观察属性
    private string _quickBuyHotkey = ""; // 快速购买快捷键

    [ObservableProperty] // 标记字段为可观察属性
    private string _quickBuyHotkeyType = HotKeyTypeEnum.KeyboardMonitor.ToString(); // 快速购买快捷键类型，默认为 KeyboardMonitor

    [ObservableProperty] // 标记字段为可观察属性
    private string _quickSereniteaPotHotkey = ""; // 快速塞壬茶壶快捷键

    [ObservableProperty] // 标记字段为可观察属性
    private string _quickSereniteaPotHotkeyType = HotKeyTypeEnum.KeyboardMonitor.ToString(); // 快速塞壬茶壶快捷键类型，默认为 KeyboardMonitor

    [ObservableProperty] // 标记字段为可观察属性
    private string _quickTeleportEnabledHotkey = ""; // 快速传送启用快捷键

    [ObservableProperty] // 标记字段为可观察属性
    private string _quickTeleportEnabledHotkeyType = HotKeyTypeEnum.KeyboardMonitor.ToString(); // 快速传送启用快捷键类型，默认为 KeyboardMonitor

    // 快捷传送触发
    [ObservableProperty] // 标记字段为可观察属性
    // 快速传送的热键设置
    private string _quickTeleportTickHotkey = "";

    // 快速传送热键类型，默认为键盘监视器
    [ObservableProperty]
    private string _quickTeleportTickHotkeyType = HotKeyTypeEnum.KeyboardMonitor.ToString();

    // 截图的热键设置
    [ObservableProperty]
    private string _takeScreenshotHotkey = "";

    // 截图热键类型，默认为键盘监视器
    [ObservableProperty]
    private string _takeScreenshotHotkeyType = HotKeyTypeEnum.KeyboardMonitor.ToString();

    // 旋转的热键设置
    [ObservableProperty]
    private string _turnAroundHotkey = "";

    // 旋转热键类型，默认为键盘监视器
    [ObservableProperty]
    private string _turnAroundHotkeyType = HotKeyTypeEnum.KeyboardMonitor.ToString();

    // 点击确认按钮的热键设置
    [ObservableProperty]
    private string _clickGenshinConfirmButtonHotkey = "";

    // 点击确认按钮热键类型，默认为键盘监视器
    [ObservableProperty]
    private string _clickGenshinConfirmButtonHotkeyType = HotKeyTypeEnum.KeyboardMonitor.ToString();

    // 点击取消按钮的热键设置
    [ObservableProperty]
    private string _clickGenshinCancelButtonHotkey = "";

    // 点击取消按钮热键类型，默认为键盘监视器
    [ObservableProperty]
    private string _clickGenshinCancelButtonHotkeyType = HotKeyTypeEnum.KeyboardMonitor.ToString();

    // 一键战斗宏的热键设置
    [ObservableProperty]
    private string _oneKeyFightHotkey = "";

    // 一键战斗宏热键类型，默认为键盘监视器
    [ObservableProperty]
    private string _oneKeyFightHotkeyType = HotKeyTypeEnum.KeyboardMonitor.ToString();

    // 地图路线录制开始/停止的热键设置
    [ObservableProperty]
    private string _mapPosRecordHotkey = "";

    // 地图路线录制热键类型，默认为键盘监视器
    [ObservableProperty]
    private string _mapPosRecordHotkeyType = HotKeyTypeEnum.KeyboardMonitor.ToString();

    // 活动音游开始/停止的热键设置
    [ObservableProperty]
    private string _autoMusicGameHotkey = "";

    // 活动音游热键类型，默认为键盘监视器
    [ObservableProperty]
    private string _autoMusicGameHotkeyType = HotKeyTypeEnum.KeyboardMonitor.ToString();

    // 自动寻路的热键设置
    [ObservableProperty]
    private string _autoTrackPathHotkey = "";

    // 自动寻路热键类型，默认为键盘监视器
    [ObservableProperty]
    private string _autoTrackPathHotkeyType = HotKeyTypeEnum.KeyboardMonitor.ToString();

    // 测试热键设置
    [ObservableProperty]
    private string _test1Hotkey = "";

    // 测试热键类型，默认为键盘监视器
    [ObservableProperty]
    private string _test1HotkeyType = HotKeyTypeEnum.KeyboardMonitor.ToString();

    // 测试2的热键设置
    [ObservableProperty]
    private string _test2Hotkey = "";

    // 测试2热键类型，默认为键盘监视器
    [ObservableProperty]
    private string _test2HotkeyType = HotKeyTypeEnum.KeyboardMonitor.ToString();

    // 路径记录开始的热键设置
    [ObservableProperty]
    private string _getBigMapPosHotkey = "";

    // 路径记录开始热键类型，默认为键盘监视器
    [ObservableProperty]
    private string _getBigMapPosHotkeyType = HotKeyTypeEnum.KeyboardMonitor.ToString();

    // 路径记录的热键设置
    [ObservableProperty]
    private string _pathRecorderHotkey = "";

    // 路径记录热键类型，默认为键盘监视器
    [ObservableProperty]
    private string _pathRecorderHotkeyType = HotKeyTypeEnum.KeyboardMonitor.ToString();

    // 添加路径记录点的热键设置
    [ObservableProperty]
    private string _addWaypointHotkey = "";

    // 添加路径记录点热键类型，默认为键盘监视器
    [ObservableProperty]
    private string _addWaypointHotkeyType = HotKeyTypeEnum.KeyboardMonitor.ToString();

    // 执行路径的热键设置
    [ObservableProperty]
    private string _executePathHotkey = "";

    // 执行路径热键类型，默认为键盘监视器
    [ObservableProperty]
    private string _executePathHotkeyType = HotKeyTypeEnum.KeyboardMonitor.ToString();

    // 日志与状态窗口展示的热键设置
    [ObservableProperty]
    // 定义一个私有字符串变量，用于存储日志框显示的热键，初始为空字符串
    private string _logBoxDisplayHotkey = "";

    // 声明一个可观察的属性，用于存储日志框显示热键的类型，初始值为 HotKeyTypeEnum.KeyboardMonitor 的字符串表示
    [ObservableProperty]
    private string _logBoxDisplayHotkeyType = HotKeyTypeEnum.KeyboardMonitor.ToString();

    // 键鼠录制/停止功能的热键
    [ObservableProperty]
    private string _keyMouseMacroRecordHotkey = "";

    // 声明一个可观察的属性，用于存储键鼠录制/停止功能热键的类型，初始值为 HotKeyTypeEnum.KeyboardMonitor 的字符串表示
    [ObservableProperty]
    private string _keyMouseMacroRecordHotkeyType = HotKeyTypeEnum.KeyboardMonitor.ToString();

    // 停止任意独立任务的热键
    [ObservableProperty]
    private string _cancelTaskHotkey = "";

    // 声明一个可观察的属性，用于存储停止任意独立任务热键的类型，初始值为 HotKeyTypeEnum.KeyboardMonitor 的字符串表示
    [ObservableProperty]
    private string _cancelTaskHotkeyType = HotKeyTypeEnum.KeyboardMonitor.ToString();
# 结束当前的代码块或控制结构
}
```