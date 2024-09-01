# `.\better-genshin-impact\Build\MicaSetup\Views\MainWindow.xaml.cs`

```cs
# 引入所需的命名空间和类
﻿using MicaSetup.Design.Controls;
using MicaSetup.Helper;
using System.ComponentModel;
using System.Windows.Media;

namespace MicaSetup.Views;

# 定义 MainWindow 类，继承自 WindowX
public partial class MainWindow : WindowX
{
    # 静态属性，获取图标的 ImageSource 对象，根据当前选项决定图标类型
    public static ImageSource? Favicon => new ImageSourceConverter().ConvertFromString($"pack://application:,,,/MicaSetup;component/Resources/Images/Favicon{(Option.Current.IsUninst ? "Uninst" : "Setup")}.ico") as ImageSource;

    # 静态属性，获取设置的名称
    public static string SetupName => Option.Current.SetupName;

    # MainWindow 类的构造函数
    public MainWindow()
    {
        # 设置数据上下文为当前实例
        DataContext = this;
        # 初始化组件
        InitializeComponent();
        # 订阅窗口关闭事件
        Closing += OnClosing;
    }

    # 处理窗口关闭事件的方法
    private void OnClosing(object sender, CancelEventArgs e)
    {
        # 如果当前是卸载模式
        if (Option.Current.IsUninst)
        {
            # 如果当前正在卸载
            if (Option.Current.Uninstalling)
            {
                # 取消关闭操作，并显示提示信息
                e.Cancel = true;
                _ = MessageBoxX.Info(this, Mui("UninstNotCompletedTips"));
            }
        }
        # 如果当前不是卸载模式
        else
        {
            # 如果当前正在安装
            if (Option.Current.Installing)
            {
                # 显示询问对话框，检查用户是否确认取消关闭
                if (MessageBoxX.Question(this, Mui("InstNotCompletedTips")) != WindowDialogResult.Yes)
                {
                    # 如果用户选择不确认，则取消关闭操作
                    e.Cancel = true;
                }
            }
        }

        # 如果没有取消关闭操作，清理临时路径
        if (!e.Cancel)
        {
            TempPathForkHelper.Clean();
        }
    }
}
```