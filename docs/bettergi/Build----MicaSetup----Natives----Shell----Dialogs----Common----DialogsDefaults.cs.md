# `.\better-genshin-impact\Build\MicaSetup\Natives\Shell\Dialogs\Common\DialogsDefaults.cs`

```cs
# 定义 MicaSetup.Shell.Dialogs 命名空间
﻿namespace MicaSetup.Shell.Dialogs;

# 定义一个内部静态类，用于存储对话框的默认值
internal static class DialogsDefaults
{
    # 定义一个内部常量，表示理想宽度（值为 0）
    internal const int IdealWidth = 0;

    # 定义进度条的最大值常量（值为 100）
    internal const int ProgressBarMaximumValue = 100;
    # 定义进度条的最小值常量（值为 0）
    internal const int ProgressBarMinimumValue = 0;
    # 定义进度条的起始值常量（值为 0）
    internal const int ProgressBarStartingValue = 0;
    # 获取对话框标题的本地化默认值
    internal static string Caption => LocalizedMessages.DialogDefaultCaption;
    # 获取对话框内容的本地化默认值
    internal static string Content => LocalizedMessages.DialogDefaultContent;
    # 获取对话框主要指令的本地化默认值
    internal static string MainInstruction => LocalizedMessages.DialogDefaultMainInstruction;
}
```