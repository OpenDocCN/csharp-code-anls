# `.\better-genshin-impact\Build\MicaSetup\ViewModels\Uninst\FinishViewModel.cs`

```cs
# 引入 CommunityToolkit.Mvvm.ComponentModel 命名空间，用于数据绑定和属性通知
﻿using CommunityToolkit.Mvvm.ComponentModel;

# 引入 CommunityToolkit.Mvvm.Input 命名空间，用于命令支持
using CommunityToolkit.Mvvm.Input;

# 引入 MicaSetup.Helper 命名空间，用于 UIDispatcherHelper
using MicaSetup.Helper;

# 引入 System.Windows 命名空间，用于 Window 和 SystemCommands
using System.Windows;

# 定义 FinishViewModel 类，并指定它是 ObservableObject 的一个部分
namespace MicaSetup.ViewModels;

# 定义 FinishViewModel 类，它继承自 ObservableObject，支持数据绑定和属性变化通知
public partial class FinishViewModel : ObservableObject
{
    # 定义 Close 方法，并通过 RelayCommand 特性将其标记为可绑定到 UI 的命令
    [RelayCommand]
    public void Close()
    {
        # 检查 UIDispatcherHelper.MainWindow 是否为 Window 类型
        if (UIDispatcherHelper.MainWindow is Window window)
        {
            # 调用 SystemCommands.CloseWindow 方法关闭窗口
            SystemCommands.CloseWindow(window);
        }
    }
}
```