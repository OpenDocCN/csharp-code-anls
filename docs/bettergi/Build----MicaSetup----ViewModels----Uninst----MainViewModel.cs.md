# `.\better-genshin-impact\Build\MicaSetup\ViewModels\Uninst\MainViewModel.cs`

```cs
# 引入社区工具包中的 MVVM 组件模型和输入处理
﻿using CommunityToolkit.Mvvm.ComponentModel;
using CommunityToolkit.Mvvm.Input;
# 引入自定义控件和帮助类
using MicaSetup.Design.Controls;
using MicaSetup.Helper;
using System;
using System.IO;
using System.Windows;

# 定义 MicaSetup 命名空间中的视图模型类
namespace MicaSetup.ViewModels;

# 定义 MainViewModel 类，继承自 ObservableObject，实现数据绑定功能
public partial class MainViewModel : ObservableObject
{
    # 声明一个可观察的属性 keepMyData，并初始化为 Option.Current.KeepMyData 的值
    [ObservableProperty]
    private bool keepMyData = Option.Current.KeepMyData;

    # 在 keepMyData 属性变化时调用此方法
    partial void OnKeepMyDataChanged(bool value)
    {
        # 更新 Option.Current.KeepMyData 为新的值
        Option.Current.KeepMyData = value;
        # 如果 keepMyData 为 false，弹出提示信息
        if (!value)
        {
            _ = MessageBoxX.Info(UIDispatcherHelper.MainWindow, Mui("NotKeepMyDataTips", Mui("KeepMyDataTips")));
        }
    }

    # MainViewModel 的构造函数
    public MainViewModel()
    {
    }

    # 定义一个 RelayCommand，关联 StartUninstall 方法
    [RelayCommand]
    private void StartUninstall()
    {
        try
        {
            # 获取准备卸载的路径信息
            UninstallDataInfo uinfo = PrepareUninstallPathHelper.GetPrepareUninstallPath(Option.Current.KeyName);

            # 更新当前安装位置
            Option.Current.InstallLocation = uinfo.InstallLocation;
            # 检查指定路径是否可写
            if (!FileWritableHelper.CheckWritable(Path.Combine(Option.Current.InstallLocation, Option.Current.ExeName)))
            {
                # 弹出信息提示，并退出方法
                _ = MessageBoxX.Info(UIDispatcherHelper.MainWindow, Mui("LockedTipsAndExitTry", Option.Current.ExeName));
                return;
            }
        }
        catch (Exception e)
        {
            # 记录错误信息
            Logger.Error(e);
        }

        # 执行下一步操作
        Routing.GoToNext();
    }

    # 定义一个 RelayCommand，关联 CancelUninstall 方法
    [RelayCommand]
    private void CancelUninstall()
    {
        # 关闭当前窗口
        SystemCommands.CloseWindow(UIDispatcherHelper.MainWindow);
    }
}
```