# `.\better-genshin-impact\Build\MicaSetup\Design\Controls\MessageBox\MessageBoxX.cs`

```cs
# 引入 MicaSetup.Helper 命名空间，系统窗口处理相关
﻿using MicaSetup.Helper;
# 引入系统窗口类
using System.Windows;

# 定义 MicaSetup.Design.Controls 命名空间
namespace MicaSetup.Design.Controls;

# 定义静态类 MessageBoxX
public static class MessageBoxX
{
    # 定义静态方法 Info，用于显示信息类型的对话框
    public static WindowDialogResult Info(DependencyObject dependencyObject, string message)
    {
        # 确定对话框的拥有者窗口，如果参数为 Window 类型，则使用该窗口；否则使用 UIDispatcherHelper.MainWindow
        Window owner = (dependencyObject is Window win ? win : dependencyObject == null ? UIDispatcherHelper.MainWindow : Window.GetWindow(dependencyObject)) ?? UIDispatcherHelper.MainWindow;

        # 创建 MessageBoxDialog 对象，设置类型为信息，并设置消息内容，然后显示对话框
        return new MessageBoxDialog()
        {
            Type = MessageBoxType.Info,
            Message = message,
        }.ShowDialog(owner);
    }

    # 定义静态方法 Question，用于显示询问类型的对话框
    public static WindowDialogResult Question(DependencyObject dependencyObject, string message)
    {
        # 确定对话框的拥有者窗口，如果参数为 Window 类型，则使用该窗口；否则使用 UIDispatcherHelper.MainWindow
        Window owner = (dependencyObject is Window win ? win : dependencyObject == null ? UIDispatcherHelper.MainWindow : Window.GetWindow(dependencyObject)) ?? UIDispatcherHelper.MainWindow;

        # 创建 MessageBoxDialog 对象，设置类型为询问，并设置消息内容，然后显示对话框
        return new MessageBoxDialog()
        {
            Type = MessageBoxType.Question,
            Message = message,
        }.ShowDialog(owner);
    }
}
```