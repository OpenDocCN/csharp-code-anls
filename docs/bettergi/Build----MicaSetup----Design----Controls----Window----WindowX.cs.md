# `.\better-genshin-impact\Build\MicaSetup\Design\Controls\Window\WindowX.cs`

```cs
﻿using CommunityToolkit.Mvvm.ComponentModel;  // 引入 CommunityToolkit 的 MVVM 组件模型
using CommunityToolkit.Mvvm.Input;  // 引入 CommunityToolkit 的输入组件
using MicaSetup.Natives;  // 引入 MicaSetup 的原生功能组件
using System;  // 引入系统功能组件
using System.Windows;  // 引入 WPF 的基础组件
using System.Windows.Controls;  // 引入 WPF 的控件组件
using System.Windows.Interop;  // 引入 WPF 的窗口互操作功能
using System.Windows.Shell;  // 引入 WPF 的任务栏功能

namespace MicaSetup.Design.Controls;  // 定义命名空间 MicaSetup.Design.Controls

[INotifyPropertyChanged]  // 表示该类会在属性更改时通知
public partial class WindowX : Window  // 定义 WindowX 类，继承自 Window
{
    public HwndSource? HwndSource { get; private set; }  // 窗口的 HwndSource 属性，用于窗口消息处理
    public nint? Handle { get; private set; }  // 窗口的句柄
    public SnapLayout SnapLayout { get; } = new();  // SnapLayout 实例，用于窗口捕捉布局

    public bool IsActivated  // 是否激活窗口的属性
    {
        get => (bool)GetValue(IsActivatedProperty);  // 获取属性值
        set => SetValue(IsActivatedProperty, value);  // 设置属性值
    }

    public static readonly DependencyProperty IsActivatedProperty = DependencyProperty.Register(nameof(IsActivated), typeof(bool), typeof(WindowX), new PropertyMetadata(false));  // 注册 IsActivated 属性

    public WindowX()  // WindowX 类的构造函数
    {
        Loaded += (s, e) =>  // 窗口加载完成后执行
        {
            HwndSource = (PresentationSource.FromVisual(this) as HwndSource)!;  // 获取窗口的 HwndSource
            HwndSource.AddHook(new HwndSourceHook(WndProc));  // 添加窗口消息处理钩子
            Handle = new WindowInteropHelper(this).Handle;  // 获取窗口句柄

            if (GetTemplateChild("PART_BtnMaximize") is Button buttonMaximize)  // 如果模板中存在名为 PART_BtnMaximize 的按钮
            {
                if (SnapLayout.IsSupported)  // 如果 SnapLayout 支持
                {
                    SnapLayout.Register(buttonMaximize);  // 注册按钮以支持 SnapLayout
                }
            }
        };
    }

    protected private virtual nint WndProc(nint hWnd, int msg, nint wParam, nint lParam, ref bool handled)  // 处理窗口消息
    {
        return SnapLayout.WndProc(hWnd, msg, wParam, lParam, ref handled);  // 将消息传递给 SnapLayout 处理
    }

    protected override void OnActivated(EventArgs e)  // 窗口激活时处理
    {
        IsActivated = true;  // 设置窗口为激活状态
        _ = WindowBackdrop.ApplyBackdrop(this, WindowBackdropType.Mica, ThemeService.Current.CurrentTheme switch  // 应用背景效果
        {
            WindowsTheme.Dark => ApplicationTheme.Dark,  // 如果当前主题为暗色
            WindowsTheme.Light => ApplicationTheme.Light,  // 如果当前主题为亮色
            WindowsTheme.Auto or _ => ApplicationTheme.Unknown,  // 自动或其他情况
        });
        base.OnActivated(e);  // 调用基类的 OnActivated 方法
    }

    protected override void OnDeactivated(EventArgs e)  // 窗口去激活时处理
    {
        IsActivated = false;  // 设置窗口为非激活状态
        base.OnDeactivated(e);  // 调用基类的 OnDeactivated 方法
    }

    protected override void OnSourceInitialized(EventArgs e)  // 窗口源初始化时处理
    {
        base.OnSourceInitialized(e);  // 调用基类的 OnSourceInitialized 方法
        WindowBackdrop.ApplyBackdrop(this, WindowBackdropType.None, ApplicationTheme.Unknown);  // 应用无背景效果
        NativeMethods.HideAllWindowButton(new WindowInteropHelper(this).Handle);  // 隐藏所有窗口按钮
    }

    public override void EndInit()  // 初始化结束时处理
    {
        ApplyResizeBorderThickness(WindowState);  // 应用调整边框厚度
        base.EndInit();  // 调用基类的 EndInit 方法
    }

    protected override void OnPropertyChanged(DependencyPropertyChangedEventArgs e)  // 属性更改时处理
    {
        if (e.Property.Name is nameof(WindowState))  // 如果更改的属性是 WindowState
        {
            ApplyResizeBorderThickness((WindowState)e.NewValue);  // 应用新的边框厚度
        }
        base.OnPropertyChanged(e);  // 调用基类的 OnPropertyChanged 方法
    }

    private void ApplyResizeBorderThickness(WindowState windowsState)  // 应用调整边框厚度的方法
    {
        # 根据窗口的状态和调整模式设置窗口的外观
        if (windowsState == WindowState.Maximized || ResizeMode == ResizeMode.NoResize || ResizeMode == ResizeMode.CanMinimize)
        {
            # 如果窗口状态为最大化，或者调整模式为不可调整大小或可以最小化，设置窗口外观
            WindowChrome.SetWindowChrome(this, new WindowChrome()
            {
                # 设置窗口标题栏的高度为0，隐藏标题栏
                CaptionHeight = 0d,
                # 设置窗口的圆角半径为8像素
                CornerRadius = new CornerRadius(8d),
                # 设置玻璃框架的厚度为-1像素（负值通常表示无边框）
                GlassFrameThickness = new Thickness(-1d),
                # 设置调整边框的厚度为0像素
                ResizeBorderThickness = new Thickness(0d),
            });
        }
        else
        {
            # 如果窗口状态不是最大化，且调整模式不符合上述条件，设置不同的窗口外观
            WindowChrome.SetWindowChrome(this, new WindowChrome()
            {
                # 设置窗口标题栏的高度为0，隐藏标题栏
                CaptionHeight = 0d,
                # 设置窗口的圆角半径为8像素
                CornerRadius = new CornerRadius(8d),
                # 设置玻璃框架的厚度为-1像素（负值通常表示无边框）
                GlassFrameThickness = new Thickness(-1d),
                # 设置调整边框的厚度为3像素
                ResizeBorderThickness = new Thickness(3d),
            });
        }
    }

    # 定义一个用于关闭窗口的命令
    [RelayCommand]
    private void CloseWindow()
    {
        # 执行关闭窗口的系统命令
        WindowSystemCommands.CloseWindow(this);
    }

    # 定义一个用于最小化窗口的命令
    [RelayCommand]
    private void MinimizeWindow()
    {
        # 执行最小化窗口的系统命令
        WindowSystemCommands.MinimizeWindow(this);
    }

    # 定义一个用于最大化窗口的命令
    [RelayCommand]
    private void MaximizeWindow()
    {
        # 执行最大化窗口的系统命令
        WindowSystemCommands.MaximizeWindow(this);
    }

    # 定义一个用于恢复窗口大小的命令
    [RelayCommand]
    private void RestoreWindow()
    {
        # 执行恢复窗口大小的系统命令
        WindowSystemCommands.RestoreWindow(this);
    }

    # 定义一个用于最大化或恢复窗口大小的命令
    [RelayCommand]
    private void MaximizeOrRestoreWindow()
    {
        # 执行最大化或恢复窗口大小的系统命令
        WindowSystemCommands.MaximizeOrRestoreWindow(this);
    }

    # 定义一个用于显示系统菜单的命令
    [RelayCommand]
    private void ShowSystemMenu()
    {
        # 执行显示系统菜单的系统命令
        WindowSystemCommands.ShowSystemMenu(this);
    }
请提供要注释的代码内容。
```