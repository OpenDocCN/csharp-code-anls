# `.\better-genshin-impact\Build\MicaSetup\Design\Controls\MessageBox\MessageBoxDialog.xaml.cs`

```cs
﻿using CommunityToolkit.Mvvm.ComponentModel; // 引入 CommunityToolkit 的 MVVM 组件模型
using CommunityToolkit.Mvvm.Input; // 引入 CommunityToolkit 的命令支持
using System.Windows; // 引入 WPF 的窗口类

namespace MicaSetup.Design.Controls; // 定义命名空间

[INotifyPropertyChanged] // 属性更改时通知
public partial class MessageBoxDialog : Window // 定义一个对话框类继承自 Window
{
    [ObservableProperty] // 声明属性用于通知界面更新
    private string message = null!; // 消息内容，初始化为 null

    [ObservableProperty] // 声明属性用于通知界面更新
    protected bool okayVisiable = true; // 确定按钮的可见性，默认可见

    [ObservableProperty] // 声明属性用于通知界面更新
    protected bool yesVisiable = false; // 是按钮的可见性，默认不可见

    [ObservableProperty] // 声明属性用于通知界面更新
    protected bool noVisiable = false; // 否按钮的可见性，默认不可见

    [ObservableProperty] // 声明属性用于通知界面更新
    protected WindowDialogResult result = WindowDialogResult.None; // 对话框结果，默认无结果

    partial void OnResultChanged(WindowDialogResult value) // 对话框结果改变时调用的部分方法
    {
        _ = value; // 占位符代码，防止编译器警告
        Close(); // 关闭对话框
    }

    [ObservableProperty] // 声明属性用于通知界面更新
    private string iconString = "\xe915"; // 图标字符串，初始化为默认图标

    [ObservableProperty] // 声明属性用于通知界面更新
    private MessageBoxType type = MessageBoxType.Info; // 对话框类型，默认信息类型

    partial void OnTypeChanged(MessageBoxType value) // 对话框类型改变时调用的部分方法
    {
        IconString = value switch // 根据对话框类型设置图标
        {
            MessageBoxType.Question => "\xe918", // 问题类型图标
            MessageBoxType.Info or _ => "\xe915", // 信息或其他类型图标
        };

        OkayVisiable = value switch // 根据对话框类型设置确定按钮的可见性
        {
            MessageBoxType.Question => false, // 问题类型隐藏确定按钮
            MessageBoxType.Info or _ => true, // 信息或其他类型显示确定按钮
        };

        YesVisiable = value switch // 根据对话框类型设置是按钮的可见性
        {
            MessageBoxType.Question => true, // 问题类型显示是按钮
            MessageBoxType.Info or _ => false, // 信息或其他类型隐藏是按钮
        };

        NoVisiable = value switch // 根据对话框类型设置否按钮的可见性
        {
            MessageBoxType.Question => true, // 问题类型显示否按钮
            MessageBoxType.Info or _ => false, // 信息或其他类型隐藏否按钮
        };
    }

    public MessageBoxDialog() // 构造函数
    {
        DataContext = this; // 设置数据上下文为当前对象
        InitializeComponent(); // 初始化组件
    }

    [RelayCommand] // 声明为命令的方法
    private void Okay() // 确定按钮命令方法
    {
        Result = WindowDialogResult.OK; // 设置对话框结果为 OK
    }

    [RelayCommand] // 声明为命令的方法
    private void Yes() // 是按钮命令方法
    {
        Result = WindowDialogResult.Yes; // 设置对话框结果为 Yes
    }

    [RelayCommand] // 声明为命令的方法
    private void No() // 否按钮命令方法
    {
        Result = WindowDialogResult.No; // 设置对话框结果为 No
    }

    public WindowDialogResult ShowDialog(Window owner) // 显示对话框并返回结果
    {
        Owner = owner; // 设置对话框的拥有者窗口
        ShowDialog(); // 显示对话框
        return Result; // 返回对话框结果
    }
}

public enum MessageBoxType // 对话框类型枚举
{
    Info, // 信息类型
    Question, // 问题类型
}

public enum WindowDialogResult // 对话框结果枚举
{
    None = 0, // 无结果
    OK = 1, // 确定
    Cancel = 2, // 取消
    Yes = 6, // 是
    No = 7 // 否
}
```