# `.\better-genshin-impact\BetterGenshinImpact\View\MainWindow.xaml.cs`

```cs
﻿using BetterGenshinImpact.Helpers.DpiAwareness; // 引入处理 DPI 适配的帮助类
using BetterGenshinImpact.ViewModel; // 引入视图模型相关的命名空间
using Microsoft.Extensions.Logging; // 引入日志记录功能的命名空间
using System; // 引入基础系统功能的命名空间
using System.Windows; // 引入 WPF 窗体相关的命名空间
using System.Windows.Media; // 引入 WPF 绘图相关的命名空间
using Wpf.Ui; // 引入 WPF UI 组件库的命名空间
using Wpf.Ui.Controls; // 引入 WPF UI 控件相关的命名空间
using Wpf.Ui.Tray.Controls; // 引入 WPF UI 托盘控件相关的命名空间

namespace BetterGenshinImpact.View; // 定义命名空间

// 定义一个主窗体类，继承自 FluentWindow 并实现 INavigationWindow 接口
public partial class MainWindow : FluentWindow, INavigationWindow
{
    // 创建一个只读的日志记录器实例，用于记录 MainWindow 类的日志
    private readonly ILogger<MainWindow> _logger = App.GetLogger<MainWindow>();

    // 定义一个只读属性，用于获取和设置视图模型
    public MainWindowViewModel ViewModel { get; }

    // 构造函数，用于初始化 MainWindow 实例
    public MainWindow(MainWindowViewModel viewModel, IPageService pageService, INavigationService navigationService, ISnackbarService snackbarService)
    {
        // 记录主窗体实例化的调试日志
        _logger.LogDebug("主窗体实例化");
        // 设置 DataContext 为传入的视图模型
        DataContext = ViewModel = viewModel;

        // 初始化窗体组件
        InitializeComponent();
        // 初始化 DPI 适配
        this.InitializeDpiAwareness();

        // 设置页面服务
        SetPageService(pageService);
        // 设置 Snackbar 服务的展示控制器
        snackbarService.SetSnackbarPresenter(SnackbarPresenter);
        // 设置导航服务的导航控制器
        navigationService.SetNavigationControl(RootNavigation);

        // 设置当前应用程序的主窗口为当前实例
        Application.Current.MainWindow = this;

        // 为 Loaded 事件添加处理程序，激活窗口
        Loaded += (s, e) => Activate();
    }

    // 重写 OnSourceInitialized 方法，以在窗口源初始化时应用系统背景
    protected override void OnSourceInitialized(EventArgs e)
    {
        base.OnSourceInitialized(e);
        // 尝试应用系统背景
        TryApplySystemBackdrop();
    }

    // 重写 OnClosed 方法，以在窗口关闭时执行清理操作
    protected override void OnClosed(EventArgs e)
    {
        // 记录主窗体退出的调试日志
        _logger.LogDebug("主窗体退出");
        base.OnClosed(e);
        // 获取 NotifyIconViewModel 服务实例，并调用 Exit 方法
        App.GetService<NotifyIconViewModel>()?.Exit();
    }

    // 处理托盘图标的左键双击事件，显示或隐藏通知图标
    private void OnNotifyIconLeftDoubleClick(NotifyIcon sender, RoutedEventArgs e)
    {
        App.GetService<NotifyIconViewModel>()?.ShowOrHide();
    }

    // 实现 INavigationWindow 接口的方法，返回导航视图
    public INavigationView GetNavigation() => RootNavigation;

    // 实现 INavigationWindow 接口的方法，导航到指定页面类型
    public bool Navigate(Type pageType) => RootNavigation.Navigate(pageType);

    // 实现 IServiceProvider 接口的方法，抛出未实现异常
    public void SetServiceProvider(IServiceProvider serviceProvider)
    {
        throw new NotImplementedException();
    }

    // 实现 INavigationWindow 接口的方法，设置页面服务
    public void SetPageService(IPageService pageService)
    {
        RootNavigation.SetPageService(pageService);
    }

    // 实现 INavigationWindow 接口的方法，显示窗口
    public void ShowWindow() => Show();

    // 实现 INavigationWindow 接口的方法，关闭窗口
    public void CloseWindow() => Close();

    // 尝试应用系统背景
    private void TryApplySystemBackdrop()
    {
        // 如果系统支持 Mica 背景，则应用 Mica 背景
        if (WindowBackdrop.IsSupported(WindowBackdropType.Mica))
        {
            Background = new SolidColorBrush(Colors.Transparent);
            WindowBackdrop.ApplyBackdrop(this, WindowBackdropType.Mica);
            return;
        }

        // 如果系统支持 Tabbed 背景，则应用 Tabbed 背景
        if (WindowBackdrop.IsSupported(WindowBackdropType.Tabbed))
        {
            Background = new SolidColorBrush(Colors.Transparent);
            WindowBackdrop.ApplyBackdrop(this, WindowBackdropType.Tabbed);
            return;
        }
    }
}
```