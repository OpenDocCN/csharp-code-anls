# `.\better-genshin-impact\BetterGenshinImpact\ViewModel\Pages\TriggerSettingsPageViewModel.cs`

```cs
# 引用 BetterGenshinImpact.Core.Config 命名空间
﻿using BetterGenshinImpact.Core.Config;
# 引用 BetterGenshinImpact.GameTask.AutoPick 命名空间
using BetterGenshinImpact.GameTask.AutoPick;
# 引用 BetterGenshinImpact.GameTask.AutoSkip.Assets 命名空间
using BetterGenshinImpact.GameTask.AutoSkip.Assets;
# 引用 BetterGenshinImpact.Service.Interface 命名空间
using BetterGenshinImpact.Service.Interface;
# 引用 BetterGenshinImpact.View.Pages 命名空间
using BetterGenshinImpact.View.Pages;
# 引用 BetterGenshinImpact.View.Windows 命名空间
using BetterGenshinImpact.View.Windows;
# 引用 CommunityToolkit.Mvvm.ComponentModel 命名空间
using CommunityToolkit.Mvvm.ComponentModel;
# 引用 CommunityToolkit.Mvvm.Input 命名空间
using CommunityToolkit.Mvvm.Input;
# 引用 System.Collections.Generic 命名空间
using System.Collections.Generic;
# 引用 System.Diagnostics 命名空间
using System.Diagnostics;
# 引用 Wpf.Ui 命名空间
using Wpf.Ui;
# 引用 Wpf.Ui.Controls 命名空间
using Wpf.Ui.Controls;

# 命名空间定义
namespace BetterGenshinImpact.ViewModel.Pages;

# 触发设置页面视图模型类，继承 ObservableObject、INavigationAware 和 IViewModel 接口
public partial class TriggerSettingsPageViewModel : ObservableObject, INavigationAware, IViewModel
{
    # 定义可观察的属性，用于存储点击聊天选项的名称
    [ObservableProperty]
    private string[] _clickChatOptionNames = ["优先选择第一个选项", "随机选择选项", "优先选择最后一个选项", "不选择选项"];

    # 定义可观察的属性，用于存储 OCR 引擎名称
    [ObservableProperty]
    private string[] _pickOcrEngineNames = [PickOcrEngineEnum.Paddle.ToString(), PickOcrEngineEnum.Yap.ToString()];

    # 定义可观察的属性，用于存储默认选择按钮名称
    [ObservableProperty]
    private string[] _defaultPickButtonNames = ["F", "E"];

    # 配置对象属性
    public AllConfig Config { get; set; }

    # 导航服务的只读字段
    private readonly INavigationService _navigationService;

    # 定义可观察的属性，用于存储社交活动分支
    [ObservableProperty]
    private List<string> _hangoutBranches;

    # 构造函数，初始化配置和导航服务
    public TriggerSettingsPageViewModel(IConfigService configService, INavigationService navigationService)
    {
        # 从配置服务中获取配置
        Config = configService.Get();
        # 初始化导航服务
        _navigationService = navigationService;
        # 获取社交活动选项标题列表
        _hangoutBranches = HangoutConfig.Instance.HangoutOptionsTitleList;
    }

    # 空实现的导航到页面的方法
    public void OnNavigatedTo()
    {
    }

    # 空实现的导航离开页面的方法
    public void OnNavigatedFrom()
    {
    }

    # 编辑黑名单的命令
    [RelayCommand]
    private void OnEditBlacklist()
    {
        # 显示 JSON 黑名单对话框
        JsonMonoDialog.Show(@"User\pick_black_lists.json");
    }

    # 编辑白名单的命令
    [RelayCommand]
    private void OnEditWhitelist()
    {
        # 显示 JSON 白名单对话框
        JsonMonoDialog.Show(@"User\pick_white_lists.json");
    }

    # 跳过的命令（注释掉了）
    # [RelayCommand]
    # private void OnOpenReExploreCharacterBox(object sender)
    # {
    #     # 提示对话框用于输入角色名
    #     var str = PromptDialog.Prompt("请使用派遣界面展示的角色名，英文逗号分割，从左往右优先级依次降低。\n示例：菲谢尔,班尼特,夜兰,申鹤,久岐忍",
    #         "派遣角色优先级配置", Config.AutoSkipConfig.AutoReExploreCharacter);
    #     # 处理用户输入的角色名字符串
    #     Config.AutoSkipConfig.AutoReExploreCharacter = str.Replace("，", ",").Replace(" ", "");
    # }

    # 跳转到QQ群链接的命令
    [RelayCommand]
    public void OnGoToQGroupUrl()
    {
        # 启动默认浏览器打开QQ群链接
        Process.Start(new ProcessStartInfo("http://qm.qq.com/cgi-bin/qm/qr?_wv=1027&k=mL1O7atys6Prlu5LBVqmDlfOrzyPMLN4&authKey=jSI2WuZyUjmpIUIAsBAf5g0r5QeSu9K6Un%2BRuSsQ8fQGYwGYwRVioFfJyYnQqvbf&noverify=0&group_code=863012276") { UseShellExecute = true });
    }

    # 跳转到快捷键页面的命令
    [RelayCommand]
    public void OnGoToHotKeyPage()
    {
        # 使用导航服务导航到快捷键页面
        _navigationService.Navigate(typeof(HotKeyPage));
    }
}
```