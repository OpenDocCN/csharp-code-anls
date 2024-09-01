# `.\better-genshin-impact\Build\MicaSetup\Natives\Shell\Dialogs\CommonFileDialogs\CommonFileDialogButton.cs`

```cs
# 引入所需的命名空间
﻿using System;
using System.Diagnostics;

# 定义命名空间 MicaSetup.Shell.Dialogs
namespace MicaSetup.Shell.Dialogs;

# 定义 CommonFileDialogButton 类，继承自 CommonFileDialogProminentControl 类
public class CommonFileDialogButton : CommonFileDialogProminentControl
{
    # 无参构造函数，调用基类构造函数
    public CommonFileDialogButton() : base(string.Empty)
    {
    }

    # 传入文本的构造函数，调用基类构造函数
    public CommonFileDialogButton(string text) : base(text)
    {
    }

    # 传入名称和文本的构造函数，调用基类构造函数
    public CommonFileDialogButton(string name, string text) : base(name, text)
    {
    }

    # 定义 Click 事件，初始化为一个空的委托
    public event EventHandler Click = delegate { };

    # 重写 Attach 方法，用于将按钮添加到对话框中
    internal override void Attach(IFileDialogCustomize dialog)
    {
        # 确保传入的对话框参数不为 null
        Debug.Assert(dialog != null, "CommonFileDialogButton.Attach: dialog parameter can not be null");

        # 向对话框中添加一个按钮，使用 Id 和 Text 属性
        dialog!.AddPushButton(Id, Text);

        # 如果按钮是显著的，将其标记为显著
        if (IsProminent) { dialog.MakeProminent(Id); }

        # 同步未管理的属性
        SyncUnmanagedProperties();
    }

    # 调用 Click 事件处理程序，如果按钮启用状态
    internal void RaiseClickEvent()
    {
        if (Enabled) { Click(this, EventArgs.Empty); }
    }
}
```