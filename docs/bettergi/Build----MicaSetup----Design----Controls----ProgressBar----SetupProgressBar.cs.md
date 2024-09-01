# `.\better-genshin-impact\Build\MicaSetup\Design\Controls\ProgressBar\SetupProgressBar.cs`

```cs
# 引入 WPF 窗体应用程序相关的命名空间
﻿using System.Windows;
using System.Windows.Controls;
using System.Windows.Shell;

# 定义命名空间
namespace MicaSetup.Design.Controls;

# 定义一个名为 SetupProgressBar 的类，继承自 ProgressBar
public class SetupProgressBar : ProgressBar
{
    # 定义一个名为 SyncToWindowTaskbar 的属性，获取和设置绑定到 LinkToWindowTaskbarProperty 的值
    public bool SyncToWindowTaskbar
    {
        get => (bool)GetValue(LinkToWindowTaskbarProperty);
        set => SetValue(LinkToWindowTaskbarProperty, value);
    }

    # 注册一个依赖属性 LinkToWindowTaskbarProperty，类型为 bool，默认值为 true
    public static readonly DependencyProperty LinkToWindowTaskbarProperty = DependencyProperty.Register("SyncToWindowTaskbar", typeof(bool), typeof(SetupProgressBar), new PropertyMetadata(true));

    # 构造函数，订阅 ValueChanged 事件
    public SetupProgressBar()
    {
        ValueChanged += OnValueChanged;
    }

    # 事件处理程序，当 ProgressBar 的值发生变化时调用
    private void OnValueChanged(object sender, RoutedPropertyChangedEventArgs<double> e)
    {
        # 如果 SyncToWindowTaskbar 为 false，退出方法
        if (!SyncToWindowTaskbar)
        {
            return;
        }

        # 查找任务栏项信息
        if (this.FindTaskbar() is TaskbarItemInfo taskbar)
        {
            # 如果值大于等于 0，则更新任务栏进度值
            if (Value >= 0d)
            {
                taskbar.ProgressValue = Value / 100d;

                # 根据进度值设置任务栏进度状态
                if (Value >= 100d)
                {
                    taskbar.ProgressState = TaskbarItemProgressState.None;
                }
                else
                {
                    taskbar.ProgressState = TaskbarItemProgressState.Normal;
                }
            }
            # 如果值小于 0，则将任务栏进度状态设置为不确定
            else
            {
                taskbar.ProgressValue = 0d;
                taskbar.ProgressState = TaskbarItemProgressState.Indeterminate;
            }
        }
    }
}
```