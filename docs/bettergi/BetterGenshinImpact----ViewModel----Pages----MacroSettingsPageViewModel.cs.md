# `.\better-genshin-impact\BetterGenshinImpact\ViewModel\Pages\MacroSettingsPageViewModel.cs`

```cs
# 导入 BetterGenshinImpact 核心配置类
using BetterGenshinImpact.Core.Config;
# 导入自动战斗任务相关类
using BetterGenshinImpact.GameTask.AutoFight;
# 导入服务接口
using BetterGenshinImpact.Service.Interface;
# 导入视图页面类
using BetterGenshinImpact.View.Pages;
# 导入视图窗口类
using BetterGenshinImpact.View.Windows;
# 导入 CommunityToolkit.Mvvm 组件模型和输入命令
using CommunityToolkit.Mvvm.ComponentModel;
using CommunityToolkit.Mvvm.Input;
# 导入系统进程相关类
using System.Diagnostics;
# 导入 Wpf.Ui 相关控件
using Wpf.Ui;
using Wpf.Ui.Controls;

# 定义命名空间 BetterGenshinImpact.ViewModel.Pages
namespace BetterGenshinImpact.ViewModel.Pages;

# 定义一个部分类 MacroSettingsPageViewModel，继承 ObservableObject, 实现 INavigationAware 和 IViewModel 接口
public partial class MacroSettingsPageViewModel : ObservableObject, INavigationAware, IViewModel
{
    # 定义一个公开的属性 Config，用于存储所有配置
    public AllConfig Config { get; set; }

    # 定义一个只读的字段 _navigationService，用于进行页面导航
    private readonly INavigationService _navigationService;

    # 使用 ObservableProperty 特性生成 _quickFightMacroHotkeyMode 属性的通知代码
    [ObservableProperty]
    # 定义一个私有的字符串数组属性 _quickFightMacroHotkeyMode，用于存储快速战斗宏的热键模式
    private string[] _quickFightMacroHotkeyMode = [OneKeyFightTask.HoldOnMode, OneKeyFightTask.TickMode];

    # 构造函数，接受 IConfigService 和 INavigationService 实例
    public MacroSettingsPageViewModel(IConfigService configService, INavigationService navigationService)
    {
        # 通过 configService 获取配置并赋值给 Config
        Config = configService.Get();
        # 初始化 _navigationService
        _navigationService = navigationService;
    }

    # 实现 INavigationAware 接口方法，导航到此页面时调用
    public void OnNavigatedTo()
    {
    }

    # 实现 INavigationAware 接口方法，导航离开此页面时调用
    public void OnNavigatedFrom()
    {
    }

    # 使用 RelayCommand 特性定义命令 OnGoToHotKeyPage
    [RelayCommand]
    # 执行导航到热键页面的操作
    public void OnGoToHotKeyPage()
    {
        # 使用 _navigationService 导航到 HotKeyPage 页面
        _navigationService.Navigate(typeof(HotKeyPage));
    }

    # 使用 RelayCommand 特性定义命令 OnEditAvatarMacro
    [RelayCommand]
    # 执行编辑头像宏文件的操作
    public void OnEditAvatarMacro()
    {
        # 显示 JsonMonoDialog 对话框来编辑用户头像宏 JSON 文件
        JsonMonoDialog.Show(@"User\avatar_macro.json");
    }

    # 使用 RelayCommand 特性定义命令 OnGoToOneKeyMacroUrl
    [RelayCommand]
    # 执行打开一键宏 URL 的操作
    public void OnGoToOneKeyMacroUrl()
    {
        # 启动默认浏览器并打开指定 URL
        Process.Start(new ProcessStartInfo("https://bgi.huiyadan.com/feats/onem.html") { UseShellExecute = true });
    }
}
```