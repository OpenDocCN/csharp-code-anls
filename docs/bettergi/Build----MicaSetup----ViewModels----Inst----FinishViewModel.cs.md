# `.\better-genshin-impact\Build\MicaSetup\ViewModels\Inst\FinishViewModel.cs`

```cs
# 引入 CommunityToolkit.Mvvm.ComponentModel 命名空间中的类和接口
using CommunityToolkit.Mvvm.ComponentModel;
# 引入 CommunityToolkit.Mvvm.Input 命名空间中的类和接口
using CommunityToolkit.Mvvm.Input;
# 引入 MicaSetup.Helper 命名空间中的类
using MicaSetup.Helper;
# 引入系统相关命名空间
using System;
using System.IO;
using System.Windows;

# 定义 MicaSetup.ViewModels 命名空间
namespace MicaSetup.ViewModels;

# 定义 FinishViewModel 类，继承自 ObservableObject，使其具备通知属性变化的能力
public partial class FinishViewModel : ObservableObject
{
    # FinishViewModel 类的构造函数
    public FinishViewModel()
    {
    }

    # 标记为 RelayCommand 的方法，使其能够绑定到 UI 命令
    [RelayCommand]
    public void Close()
    {
        # 检查 UIDispatcherHelper.MainWindow 是否是 Window 类型
        if (UIDispatcherHelper.MainWindow is Window window)
        {
            # 通过系统命令关闭窗口
            SystemCommands.CloseWindow(window);
        }
    }

    # 标记为 RelayCommand 的方法，使其能够绑定到 UI 命令
    [RelayCommand]
    public void Open()
    {
        # 检查 UIDispatcherHelper.MainWindow 是否是 Window 类型
        if (UIDispatcherHelper.MainWindow is Window window)
        {
            try
            {
                # 创建一个新的进程，指定启动的文件、工作目录和其他参数
                FluentProcess.Create()
                    .FileName(Path.Combine(Option.Current.InstallLocation, Option.Current.ExeName)) # 设置启动的文件路径
                    .WorkingDirectory(Option.Current.InstallLocation) # 设置工作目录
                    .UseShellExecute() # 使用操作系统外壳启动进程
                    .Start() # 启动进程
                    .Forget(); # 忽略返回值，表示不关心进程启动是否成功
            }
            catch (Exception e)
            {
                # 记录异常信息
                Logger.Error(e);
            }
            # 关闭窗口
            SystemCommands.CloseWindow(window);
        }
    }
}
```