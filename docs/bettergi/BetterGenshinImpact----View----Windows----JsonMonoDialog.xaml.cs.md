# `.\better-genshin-impact\BetterGenshinImpact\View\Windows\JsonMonoDialog.xaml.cs`

```cs
# 引用相关命名空间
﻿using BetterGenshinImpact.ViewModel.Windows;
using System;
using System.Windows;
using System.Windows.Media;
using Wpf.Ui.Controls;

namespace BetterGenshinImpact.View.Windows;

# 定义一个继承自 FluentWindow 的类 JsonMonoDialog
public partial class JsonMonoDialog : FluentWindow
{
    # 定义一个只读属性 ViewModel，类型为 JsonMonoViewModel
    public JsonMonoViewModel ViewModel { get; }

    # 构造函数，接受一个路径参数
    public JsonMonoDialog(string path)
    {
        # 设置 DataContext 为新的 JsonMonoViewModel 实例，并初始化 ViewModel
        DataContext = ViewModel = new(path);
        # 初始化窗口组件
        InitializeComponent();

        # 手动绑定 MVVM，更新 ViewModel 的 JsonText 属性
        JsonCodeBox.TextChanged += (_, _) => ViewModel.JsonText = JsonCodeBox.Text;
    }

    # 重写 OnSourceInitialized 方法，以在窗口源初始化时应用系统背景
    protected override void OnSourceInitialized(EventArgs e)
    {
        base.OnSourceInitialized(e);
        TryApplySystemBackdrop();
    }

    # 尝试应用系统背景
    private void TryApplySystemBackdrop()
    {
        # 如果 Mica 背景支持，则应用 Mica 背景
        if (WindowBackdrop.IsSupported(WindowBackdropType.Mica))
        {
            Background = new SolidColorBrush(Colors.Transparent);
            WindowBackdrop.ApplyBackdrop(this, WindowBackdropType.Mica);
            return;
        }

        # 如果 Tabbed 背景支持，则应用 Tabbed 背景
        if (WindowBackdrop.IsSupported(WindowBackdropType.Tabbed))
        {
            Background = new SolidColorBrush(Colors.Transparent);
            WindowBackdrop.ApplyBackdrop(this, WindowBackdropType.Tabbed);
        }
    }

    # 静态方法，显示一个 JsonMonoDialog 对话框
    public static void Show(string path)
    {
        JsonMonoDialog dialog = new(path)
        {
            # 设置对话框的拥有者为当前应用的主窗口
            Owner = Application.Current.MainWindow
        };
        # 显示对话框
        dialog.Show();
    }
}
```