# `.\better-genshin-impact\BetterGenshinImpact\ViewModel\Pages\TaskSettingsPageViewModel.cs`

```cs
# 导入 BetterGenshinImpact 核心配置相关类
using BetterGenshinImpact.Core.Config;
# 导入 BetterGenshinImpact 游戏任务相关类
using BetterGenshinImpact.GameTask;
using BetterGenshinImpact.GameTask.AutoDomain;
using BetterGenshinImpact.GameTask.AutoFight;
using BetterGenshinImpact.GameTask.AutoGeniusInvokation;
using BetterGenshinImpact.GameTask.AutoMusicGame;
using BetterGenshinImpact.GameTask.AutoSkip.Model;
using BetterGenshinImpact.GameTask.AutoTrackPath;
using BetterGenshinImpact.GameTask.AutoWood;
using BetterGenshinImpact.GameTask.Model;
# 导入 BetterGenshinImpact 服务接口
using BetterGenshinImpact.Service.Interface;
# 导入 BetterGenshinImpact 视图页面相关类
using BetterGenshinImpact.View.Pages;
# 导入 CommunityToolkit.Mvvm 组件模型和输入相关类
using CommunityToolkit.Mvvm.ComponentModel;
using CommunityToolkit.Mvvm.Input;
# 导入系统相关命名空间
using System;
using System.IO;
using System.Threading;
using System.Threading.Tasks;
# 导入 Windows 系统相关命名空间
using Windows.System;
# 导入 Wpf.Ui 控件相关命名空间
using Wpf.Ui;
using Wpf.Ui.Controls;
using Wpf.Ui.Violeta.Controls;

# 定义命名空间
namespace BetterGenshinImpact.ViewModel.Pages;

# 定义任务设置页面视图模型类，继承 ObservableObject，实现 INavigationAware 和 IViewModel 接口
public partial class TaskSettingsPageViewModel : ObservableObject, INavigationAware, IViewModel
{
    # 配置属性
    public AllConfig Config { get; set; }

    # 私有只读导航服务
    private readonly INavigationService _navigationService;
    # 私有只读任务触发分发器
    private readonly TaskTriggerDispatcher _taskDispatcher;

    # 取消操作令牌源
    private CancellationTokenSource? _cts;
    # 静态只读锁对象
    private static readonly object _locker = new();

    # 观察属性策略列表
    [ObservableProperty] private string[] _strategyList;
    # 观察属性自动天赋启用按钮文本
    [ObservableProperty] private string _switchAutoGeniusInvokationButtonText = "启动";

    # 观察属性自动木材回合数
    [ObservableProperty] private int _autoWoodRoundNum;
    # 观察属性自动木材每日最大数量
    [ObservableProperty] private int _autoWoodDailyMaxCount = 2000;
    # 观察属性自动木材启用按钮文本
    [ObservableProperty] private string _switchAutoWoodButtonText = "启动";

    # 观察属性战斗策略列表
    [ObservableProperty] private string[] _combatStrategyList;
    # 观察属性自动领域回合数
    [ObservableProperty] private int _autoDomainRoundNum;
    # 观察属性自动领域启用按钮文本
    [ObservableProperty] private string _switchAutoDomainButtonText = "启动";
    # 观察属性自动战斗启用按钮文本
    [ObservableProperty] private string _switchAutoFightButtonText = "启动";
    # 观察属性自动追踪启用按钮文本
    [ObservableProperty] private string _switchAutoTrackButtonText = "启动";
    # 观察属性自动路径追踪启用按钮文本
    [ObservableProperty] private string _switchAutoTrackPathButtonText = "启动";
    # 观察属性自动音乐游戏启用按钮文本
    [ObservableProperty] private string _switchAutoMusicGameButtonText = "启动";

    # 任务设置页面视图模型构造函数，注入配置服务、导航服务和任务触发分发器
    public TaskSettingsPageViewModel(IConfigService configService, INavigationService navigationService, TaskTriggerDispatcher taskTriggerDispatcher)
    {
        # 从配置服务获取配置
        Config = configService.Get();
        # 初始化导航服务
        _navigationService = navigationService;
        # 初始化任务触发分发器
        _taskDispatcher = taskTriggerDispatcher;

        # 加载自定义脚本到策略列表
        _strategyList = LoadCustomScript(Global.Absolute(@"User\AutoGeniusInvokation"));

        # 加载战斗策略列表，包含根据队伍自动选择和自定义战斗脚本
        _combatStrategyList = ["根据队伍自动选择", .. LoadCustomScript(Global.Absolute(@"User\AutoFight"))];
    }

    # 加载自定义脚本方法，返回字符串数组
    private string[] LoadCustomScript(string folder)
    # 获取指定文件夹下的所有文件，包含子文件夹中的文件
    var files = Directory.GetFiles(folder, "*.*",
        SearchOption.AllDirectories);

    # 创建一个与文件数量相同的字符串数组
    var strategyList = new string[files.Length];
    # 遍历所有文件
    for (var i = 0; i < files.Length; i++)
    {
        # 如果文件名以 ".txt" 结尾
        if (files[i].EndsWith(".txt"))
        {
            # 去掉文件夹路径和文件扩展名，获取策略名
            var strategyName = files[i].Replace(folder, "").Replace(".txt", "");
            # 如果策略名以反斜杠开头，去掉这个反斜杠
            if (strategyName.StartsWith('\\'))
            {
                strategyName = strategyName[1..];
            }
            # 将策略名保存到策略列表中
            strategyList[i] = strategyName;
        }
    }

    # 返回策略名称列表
    return strategyList;
}

# RelayCommand 特性表示这是一个命令
[RelayCommand]
# 当策略下拉框打开时执行的方法
private void OnStrategyDropDownOpened(string type)
{
    # 根据策略类型来决定如何加载策略列表
    switch (type)
    {
        # 如果策略类型是 "Combat"
        case "Combat":
            # 设置战斗策略列表，包含自动选择和自定义脚本
            CombatStrategyList = ["根据队伍自动选择", .. LoadCustomScript(Global.Absolute(@"User\AutoFight"))];
            break;

        # 如果策略类型是 "GeniusInvocation"
        case "GeniusInvocation":
            # 加载自定义脚本到策略列表
            StrategyList = LoadCustomScript(Global.Absolute(@"User\AutoGeniusInvokation"));
            break;
    }
}

# 当导航到此页面时执行的方法
public void OnNavigatedTo()
{
}

# 当离开此页面时执行的方法
public void OnNavigatedFrom()
{
}

# RelayCommand 特性表示这是一个命令
[RelayCommand]
# 当用户点击跳转到热键页面时执行的方法
public void OnGoToHotKeyPage()
{
    # 导航到热键页面
    _navigationService.Navigate(typeof(HotKeyPage));
}

# RelayCommand 特性表示这是一个命令
[RelayCommand]
# 切换自动天才召唤策略的开关
public void OnSwitchAutoGeniusInvokation()
{
    try
    {
        # 锁定以防多线程访问冲突
        lock (_locker)
        {
            # 如果当前按钮文本是 "启动"
            if (SwitchAutoGeniusInvokationButtonText == "启动")
            {
                # 如果策略名称为空，弹出警告消息
                if (string.IsNullOrEmpty(Config.AutoGeniusInvokationConfig.StrategyName))
                {
                    Toast.Warning("请先选择策略");
                    return;
                }

                # 获取策略文件的完整路径
                var path = Global.Absolute(@"User\AutoGeniusInvokation\" + Config.AutoGeniusInvokationConfig.StrategyName + ".txt");

                # 如果策略文件不存在，弹出错误消息
                if (!File.Exists(path))
                {
                    Toast.Error("策略文件不存在");
                    return;
                }

                # 读取策略文件内容
                var content = File.ReadAllText(path);
                # 取消之前的任务
                _cts?.Cancel();
                # 创建新的取消令牌源
                _cts = new CancellationTokenSource();
                # 创建任务参数并启动独立任务
                var param = new GeniusInvokationTaskParam(_cts, content);
                _taskDispatcher.StartIndependentTask(IndependentTaskEnum.AutoGeniusInvokation, param);
                # 更新按钮文本为 "停止"
                SwitchAutoGeniusInvokationButtonText = "停止";
            }
            else
            {
                # 如果按钮文本是 "停止"，取消任务并重置按钮文本为 "启动"
                _cts?.Cancel();
                SwitchAutoGeniusInvokationButtonText = "启动";
            }
        }
    }
    catch (Exception ex)
    {
        # 捕获异常并显示错误消息
        MessageBox.Error(ex.Message);
    }
}

# RelayCommand 特性表示这是一个命令
[RelayCommand]
# 异步方法，打开自动天才召唤的文档链接
public async Task OnGoToAutoGeniusInvokationUrlAsync()
{
    # 启动浏览器打开指定的 URL
    await Launcher.LaunchUriAsync(new Uri("https://bgi.huiyadan.com/doc.html#%E8%87%AA%E5%8A%A8%E4%B8%83%E5%9C%A3%E5%8F%AC%E5%94%A4"));
}

# RelayCommand 特性表示这是一个命令
[RelayCommand]
public void OnSwitchAutoWood()
    {
        try
        {
            // 使用互斥锁确保在同一时间只有一个线程可以执行此代码块
            lock (_locker)
            {
                // 如果按钮文本为 "启动"，则执行以下操作
                if (SwitchAutoWoodButtonText == "启动")
                {
                    // 如果存在取消令牌，则取消当前操作
                    _cts?.Cancel();
                    // 创建新的取消令牌源
                    _cts = new CancellationTokenSource();
                    // 创建新的木材任务参数对象
                    var param = new WoodTaskParam(_cts, AutoWoodRoundNum, AutoWoodDailyMaxCount);
                    // 启动独立的木材任务
                    _taskDispatcher.StartIndependentTask(IndependentTaskEnum.AutoWood, param);
                    // 更新按钮文本为 "停止"
                    SwitchAutoWoodButtonText = "停止";
                }
                else
                {
                    // 如果按钮文本不为 "启动"，则取消当前操作
                    _cts?.Cancel();
                    // 更新按钮文本为 "启动"
                    SwitchAutoWoodButtonText = "启动";
                }
            }
        }
        catch (Exception ex)
        {
            // 捕捉异常并显示错误信息
            MessageBox.Error(ex.Message);
        }
    }

    [RelayCommand]
    // 异步方法，打开指定的网页
    public async Task OnGoToAutoWoodUrlAsync()
    {
        // 异步启动 URI，打开木材自动化网页
        await Launcher.LaunchUriAsync(new Uri("https://bgi.huiyadan.com/feats/felling.html"));
    }

    [RelayCommand]
    // 方法切换自动战斗状态
    public void OnSwitchAutoFight()
    {
        try
        {
            // 使用互斥锁确保在同一时间只有一个线程可以执行此代码块
            lock (_locker)
            {
                // 如果按钮文本为 "启动"，则执行以下操作
                if (SwitchAutoFightButtonText == "启动")
                {
                    // 构建战斗策略文件路径
                    var path = Global.Absolute(@"User\AutoFight\" + Config.AutoFightConfig.StrategyName + ".txt");
                    // 如果策略名称为 "根据队伍自动选择"，则设置为目录路径
                    if ("根据队伍自动选择".Equals(Config.AutoFightConfig.StrategyName))
                    {
                        path = Global.Absolute(@"User\AutoFight\");
                    }
                    // 检查战斗策略文件或目录是否存在
                    if (!File.Exists(path) && !Directory.Exists(path))
                    {
                        // 如果文件或目录不存在，显示错误消息
                        Toast.Error("战斗策略文件不存在");
                        return;
                    }

                    // 如果存在取消令牌，则取消当前操作
                    _cts?.Cancel();
                    // 创建新的取消令牌源
                    _cts = new CancellationTokenSource();
                    // 创建新的自动战斗任务参数对象
                    var param = new AutoFightParam(_cts, path);
                    // 启动独立的自动战斗任务
                    _taskDispatcher.StartIndependentTask(IndependentTaskEnum.AutoFight, param);
                    // 更新按钮文本为 "停止"
                    SwitchAutoFightButtonText = "停止";
                }
                else
                {
                    // 如果按钮文本不为 "启动"，则取消当前操作
                    _cts?.Cancel();
                    // 更新按钮文本为 "启动"
                    SwitchAutoFightButtonText = "启动";
                }
            }
        }
        catch (Exception ex)
        {
            // 捕捉异常并显示错误信息
            MessageBox.Error(ex.Message);
        }
    }

    [Obsolete]
    // 读取战斗策略，已废弃的方法
    private string? ReadFightStrategy(string strategyName)
    {
        // 检查战斗策略名称是否为空或未指定
        if (string.IsNullOrEmpty(strategyName))
        {
            // 显示警告消息
            Toast.Warning("请先选择战斗策略");
            return null;
        }

        // 构建战斗策略文件路径
        var path = Global.Absolute(@"User\AutoFight\" + strategyName + ".txt");

        // 检查战斗策略文件是否存在
        if (!File.Exists(path))
        {
            // 如果文件不存在，显示错误消息
            Toast.Error("战斗策略文件不存在");
            return null;
        }

        // 读取并返回战斗策略文件内容
        var content = File.ReadAllText(path);
        return content;
    }

    [RelayCommand]
    // 异步方法，打开指定的网页
    public async Task OnGoToAutoFightUrlAsync()
    {
        // 异步启动 URI，打开自动战斗网页
        await Launcher.LaunchUriAsync(new Uri("https://bgi.huiyadan.com/feats/domain.html"));
    }

    [RelayCommand]
    // 方法切换自动领域状态
    public void OnSwitchAutoDomain()
    {
        try
        {
            // 进入锁定区域，确保线程安全
            lock (_locker)
            {
                // 如果按钮文本为“启动”
                if (SwitchAutoDomainButtonText == "启动")
                {
                    // 构建策略文件路径
                    var path = Global.Absolute(@"User\AutoFight\" + Config.AutoFightConfig.StrategyName + ".txt");
                    // 如果策略名称为“根据队伍自动选择”，则路径为目录而不是文件
                    if ("根据队伍自动选择".Equals(Config.AutoFightConfig.StrategyName))
                    {
                        path = Global.Absolute(@"User\AutoFight\");
                    }
                    // 如果路径对应的文件或目录不存在，显示错误消息并返回
                    if (!File.Exists(path) && !Directory.Exists(path))
                    {
                        Toast.Error("战斗策略文件不存在");
                        return;
                    }

                    // 取消之前的任务，如果存在的话
                    _cts?.Cancel();
                    // 创建新的取消令牌源
                    _cts = new CancellationTokenSource();
                    // 创建新的自动域参数对象
                    var param = new AutoDomainParam(_cts, AutoDomainRoundNum, path);
                    // 启动独立任务
                    _taskDispatcher.StartIndependentTask(IndependentTaskEnum.AutoDomain, param);
                    // 更新按钮文本为“停止”
                    SwitchAutoDomainButtonText = "停止";
                }
                else
                {
                    // 如果按钮文本不为“启动”，则取消之前的任务（如果存在）
                    _cts?.Cancel();
                    // 更新按钮文本为“启动”
                    SwitchAutoDomainButtonText = "启动";
                }
            }
        }
        catch (Exception ex)
        {
            // 捕获异常并显示错误消息
            MessageBox.Error(ex.Message);
        }
    }

    [RelayCommand]
    public async Task OnGoToAutoDomainUrlAsync()
    {
        // 异步启动浏览器并打开指定的 URL
        await Launcher.LaunchUriAsync(new Uri("https://bgi.huiyadan.com/feats/domain.html"));
    }

    [RelayCommand]
    public void OnSwitchAutoTrack()
    {
        try
        {
            // 进入锁定区域，确保线程安全
            lock (_locker)
            {
                // 如果按钮文本为“启动”
                if (SwitchAutoTrackButtonText == "启动")
                {
                    // 取消之前的任务，如果存在的话
                    _cts?.Cancel();
                    // 创建新的取消令牌源
                    _cts = new CancellationTokenSource();
                    // 创建新的自动追踪参数对象
                    var param = new AutoTrackParam(_cts);
                    // 启动独立任务
                    _taskDispatcher.StartIndependentTask(IndependentTaskEnum.AutoTrack, param);
                    // 更新按钮文本为“停止”
                    SwitchAutoTrackButtonText = "停止";
                }
                else
                {
                    // 如果按钮文本不为“启动”，则取消之前的任务（如果存在）
                    _cts?.Cancel();
                    // 更新按钮文本为“启动”
                    SwitchAutoTrackButtonText = "启动";
                }
            }
        }
        catch (Exception ex)
        {
            // 捕获异常并显示错误消息
            MessageBox.Error(ex.Message);
        }
    }

    [RelayCommand]
    public async Task OnGoToAutoTrackUrlAsync()
    {
        // 异步启动浏览器并打开指定的 URL
        await Launcher.LaunchUriAsync(new Uri("https://bgi.huiyadan.com/feats/track.html"));
    }

    [RelayCommand]
    public void OnSwitchAutoTrackPath()
    # 尝试执行以下代码块
    {
        try
        {
            # 获取锁，以确保线程安全
            lock (_locker)
            {
                # 如果当前按钮文本为“启动”
                if (SwitchAutoTrackPathButtonText == "启动")
                {
                    # 取消当前的取消令牌源（如果存在）
                    _cts?.Cancel();
                    # 创建新的取消令牌源
                    _cts = new CancellationTokenSource();
                    # 使用新的取消令牌源创建自动追踪路径参数对象
                    var param = new AutoTrackPathParam(_cts);
                    # 启动独立任务，使用自动追踪路径参数
                    _taskDispatcher.StartIndependentTask(IndependentTaskEnum.AutoTrackPath, param);
                    # 将按钮文本更改为“停止”
                    SwitchAutoTrackPathButtonText = "停止";
                }
                else
                {
                    # 取消当前的取消令牌源（如果存在）
                    _cts?.Cancel();
                    # 将按钮文本更改为“启动”
                    SwitchAutoTrackPathButtonText = "启动";
                }
            }
        }
        catch (Exception ex)
        {
            # 显示错误消息
            MessageBox.Error(ex.Message);
        }
    }

    # 声明一个异步命令，用于打开自动追踪路径的 URL
    [RelayCommand]
    public async Task OnGoToAutoTrackPathUrlAsync()
    {
        # 异步打开指定的 URI
        await Launcher.LaunchUriAsync(new Uri("https://bgi.huiyadan.com/feats/track.html"));
    }

    # 声明一个命令，用于切换自动音乐游戏的状态
    [RelayCommand]
    public void OnSwitchAutoMusicGame()
    {
        try
        {
            # 获取锁，以确保线程安全
            lock (_locker)
            {
                # 如果当前按钮文本为“启动”
                if (SwitchAutoMusicGameButtonText == "启动")
                {
                    # 取消当前的取消令牌源（如果存在）
                    _cts?.Cancel();
                    # 创建新的取消令牌源
                    _cts = new CancellationTokenSource();
                    # 使用新的取消令牌源创建自动音乐游戏参数对象
                    var param = new AutoMusicGameParam(_cts);
                    # 启动独立任务，使用自动音乐游戏参数
                    _taskDispatcher.StartIndependentTask(IndependentTaskEnum.AutoMusicGame, param);
                    # 将按钮文本更改为“停止”
                    SwitchAutoMusicGameButtonText = "停止";
                }
                else
                {
                    # 取消当前的取消令牌源（如果存在）
                    _cts?.Cancel();
                    # 将按钮文本更改为“启动”
                    SwitchAutoMusicGameButtonText = "启动";
                }
            }
        }
        catch (Exception ex)
        {
            # 显示错误消息
            MessageBox.Error(ex.Message);
        }
    }

    # 声明一个异步命令，用于打开自动音乐游戏的 URL
    [RelayCommand]
    public async Task OnGoToAutoMusicGameUrlAsync()
    {
        # 异步打开指定的 URI
        await Launcher.LaunchUriAsync(new Uri("https://bgi.huiyadan.com/feats/music.html"));
    }

    # 设置自动追踪按钮的文本，根据运行状态确定文本
    public static void SetSwitchAutoTrackButtonText(bool running)
    {
        # 获取 TaskSettingsPageViewModel 实例
        var instance = App.GetService<TaskSettingsPageViewModel>();
        # 如果实例为空，则返回
        if (instance == null)
        {
            return;
        }

        # 根据运行状态设置按钮文本
        instance.SwitchAutoTrackButtonText = running ? "停止" : "启动";
    }

    # 设置自动天才调用按钮的文本，根据运行状态确定文本
    public static void SetSwitchAutoGeniusInvokationButtonText(bool running)
    {
        # 获取 TaskSettingsPageViewModel 实例
        var instance = App.GetService<TaskSettingsPageViewModel>();
        # 如果实例为空，则返回
        if (instance == null)
        {
            return;
        }

        # 根据运行状态设置按钮文本
        instance.SwitchAutoGeniusInvokationButtonText = running ? "停止" : "启动";
    }

    # 设置自动木材按钮的文本，根据运行状态确定文本
    public static void SetSwitchAutoWoodButtonText(bool running)
    {
        # 获取 TaskSettingsPageViewModel 实例
        var instance = App.GetService<TaskSettingsPageViewModel>();
        # 如果实例为空，则返回
        if (instance == null)
        {
            return;
        }

        # 根据运行状态设置按钮文本
        instance.SwitchAutoWoodButtonText = running ? "停止" : "启动";
    }

    # 设置自动领域按钮的文本（代码未完全，注释为示例）
    public static void SetSwitchAutoDomainButtonText(bool running)
    # 获取 TaskSettingsPageViewModel 实例
    var instance = App.GetService<TaskSettingsPageViewModel>();
    # 如果实例为 null，则直接返回，不执行后续代码
    if (instance == null)
    {
        return;
    }

    # 根据 running 的值设置 SwitchAutoDomainButtonText 的文本，若 running 为 true，则设置为 "停止"，否则设置为 "启动"
    instance.SwitchAutoDomainButtonText = running ? "停止" : "启动";
}

# 定义一个静态方法来设置 SwitchAutoFightButtonText 的文本
public static void SetSwitchAutoFightButtonText(bool running)
{
    # 获取 TaskSettingsPageViewModel 实例
    var instance = App.GetService<TaskSettingsPageViewModel>();
    # 如果实例为 null，则直接返回，不执行后续代码
    if (instance == null)
    {
        return;
    }

    # 根据 running 的值设置 SwitchAutoFightButtonText 的文本，若 running 为 true，则设置为 "停止"，否则设置为 "启动"
    instance.SwitchAutoFightButtonText = running ? "停止" : "启动";
}
这个代码片段是一个单独的右大括号 `}`，通常用于结束一个代码块。请问你有具体的代码需要注释，还是这是一个示例？
```