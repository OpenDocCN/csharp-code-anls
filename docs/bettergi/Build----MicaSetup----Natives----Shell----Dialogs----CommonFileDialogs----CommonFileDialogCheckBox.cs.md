# `.\better-genshin-impact\Build\MicaSetup\Natives\Shell\Dialogs\CommonFileDialogs\CommonFileDialogCheckBox.cs`

```cs
﻿using System;
using System.Diagnostics;

namespace MicaSetup.Shell.Dialogs;

public class CommonFileDialogCheckBox : CommonFileDialogProminentControl
{
    // 定义一个私有字段来存储复选框的选中状态
    private bool isChecked;

    // 默认构造函数
    public CommonFileDialogCheckBox()
    {
    }

    // 带文本参数的构造函数，调用基类构造函数
    public CommonFileDialogCheckBox(string text) : base(text)
    {
    }

    // 带名称和文本参数的构造函数，调用基类构造函数
    public CommonFileDialogCheckBox(string name, string text) : base(name, text)
    {
    }

    // 带文本和选中状态参数的构造函数，调用基类构造函数，并设置选中状态
    public CommonFileDialogCheckBox(string text, bool isChecked)
        : base(text) => this.isChecked = isChecked;

    // 带名称、文本和选中状态参数的构造函数，调用基类构造函数，并设置选中状态
    public CommonFileDialogCheckBox(string name, string text, bool isChecked)
        : base(name, text) => this.isChecked = isChecked;

    // 定义一个事件，当复选框状态变化时触发
    public event EventHandler CheckedChanged = delegate { };

    // 复选框的选中状态属性
    public bool IsChecked
    {
        // 获取复选框的选中状态
        get => isChecked;
        set
        {
            // 如果选中状态发生变化
            if (isChecked != value)
            {
                isChecked = value; // 更新选中状态
                ApplyPropertyChange("IsChecked"); // 应用属性更改
            }
        }
    }

    // 重写 Attach 方法来附加复选框到文件对话框中
    internal override void Attach(IFileDialogCustomize dialog)
    {
        // 确保 dialog 参数不为 null
        Debug.Assert(dialog != null, "CommonFileDialogCheckBox.Attach: dialog parameter can not be null");

        // 将复选框添加到对话框中
        dialog!.AddCheckButton(Id, Text, isChecked);

        // 如果复选框是显著的，则使其显著
        if (IsProminent) { dialog.MakeProminent(Id); }

        // 应用复选框的属性更改
        ApplyPropertyChange("IsChecked");

        // 同步未管理的属性
        SyncUnmanagedProperties();
    }

    // 触发 CheckedChanged 事件
    internal void RaiseCheckedChangedEvent()
    {
        // 如果复选框启用，则触发事件
        if (Enabled)
        {
            CheckedChanged(this, EventArgs.Empty);
        }
    }
}
```