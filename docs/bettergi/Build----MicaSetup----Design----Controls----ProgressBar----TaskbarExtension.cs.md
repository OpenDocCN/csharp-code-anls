# `.\better-genshin-impact\Build\MicaSetup\Design\Controls\ProgressBar\TaskbarExtension.cs`

```cs
# 使用 Windows Shell 相关功能
﻿using System.Windows;
using System.Windows.Shell;

namespace MicaSetup.Design.Controls;

# 扩展方法类，用于处理任务栏相关功能
public static class TaskbarExtension
{
    # 查找指定元素的任务栏信息
    public static TaskbarItemInfo FindTaskbar(this FrameworkElement owner)
    {
        # 检查元素是否是窗口，若不是则获取窗口
        if ((owner is Window ? owner as Window : Window.GetWindow(owner)) is Window win)
        {
            # 如果窗口已有任务栏信息，则返回；否则创建新实例
            return win.TaskbarItemInfo ??= new();
        }
        # 若未找到窗口，返回 null
        return null!;
    }

    # 查找指定窗口的任务栏信息
    public static TaskbarItemInfo FindTaskbar(this Window owner)
    {
        # 如果窗口已有任务栏信息，则返回；否则创建新实例
        return owner.TaskbarItemInfo ??= new();
    }

    # 设置任务栏进度条的进度和状态
    public static void SetProgress(this TaskbarItemInfo taskbarItem, double value, TaskbarItemProgressState state = TaskbarItemProgressState.Normal)
    {
        # 设置进度条状态
        taskbarItem.ProgressState = state;
        # 设置进度条的值
        taskbarItem.ProgressValue = value;
    }
}
```