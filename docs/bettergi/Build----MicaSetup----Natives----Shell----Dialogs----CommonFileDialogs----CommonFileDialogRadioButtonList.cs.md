# `.\better-genshin-impact\Build\MicaSetup\Natives\Shell\Dialogs\CommonFileDialogs\CommonFileDialogRadioButtonList.cs`

```cs
﻿using System;
using System.Collections.ObjectModel;
using System.Diagnostics;
using System.Diagnostics.CodeAnalysis;
using System.Windows.Markup;

namespace MicaSetup.Shell.Dialogs;

// 指定 ContentProperty 属性为 "Items"，用于 XAML 的内容属性绑定
[ContentProperty("Items")]
// 定义一个用于对话框的单选按钮列表控件类
public class CommonFileDialogRadioButtonList : CommonFileDialogControl, ICommonFileDialogIndexedControls
{
    // 创建一个保存单选按钮项的集合
    private readonly Collection<CommonFileDialogRadioButtonListItem> items = new Collection<CommonFileDialogRadioButtonListItem>();
    // 记录当前选中的索引，初始值为 -1，表示没有选中
    private int selectedIndex = -1;

    // 默认构造函数
    public CommonFileDialogRadioButtonList()
    {
    }

    // 带名称的构造函数，调用基类构造函数
    public CommonFileDialogRadioButtonList(string name) : base(name, string.Empty)
    {
    }

    // 定义选中索引变化事件
    public event EventHandler SelectedIndexChanged = delegate { };

    // 提供对单选按钮项集合的访问
    public Collection<CommonFileDialogRadioButtonListItem> Items => items;

    // 选中索引属性，设置时触发事件或抛出异常
    [SuppressMessage("Microsoft.Usage", "CA2201:")]
    public int SelectedIndex
    {
        get => selectedIndex;
        set
        {
            // 如果新值与当前值相同，则不做任何操作
            if (selectedIndex == value) { return; }

            // 如果对话框未初始化，直接更新选中索引
            if (HostingDialog == null)
            {
                selectedIndex = value;
            }
            // 如果新值在有效范围内，更新选中索引并应用更改
            else if (value >= 0 && value < items.Count)
            {
                selectedIndex = value;
                ApplyPropertyChange("SelectedIndex");
            }
            // 否则抛出索引超出范围的异常
            else
            {
                throw new IndexOutOfRangeException(LocalizedMessages.RadioButtonListIndexOutOfBounds);
            }
        }
    }

    // 实现 ICommonFileDialogIndexedControls 接口中的方法，触发选中索引变化事件
    void ICommonFileDialogIndexedControls.RaiseSelectedIndexChangedEvent()
    {
        // 如果控件启用，则触发选中索引变化事件
        if (Enabled) { SelectedIndexChanged(this, EventArgs.Empty); }
    }

    // 覆盖基类方法，附加对话框自定义项
    internal override void Attach(IFileDialogCustomize dialog)
    {
        // 确保对话框对象不为 null
        Debug.Assert(dialog != null, "CommonFileDialogRadioButtonList.Attach: dialog parameter can not be null");

        // 将单选按钮列表添加到对话框
        dialog!.AddRadioButtonList(Id);

        // 将每个单选按钮项添加到对话框
        for (var index = 0; index < items.Count; index++)
        {
            dialog.AddControlItem(Id, index, items[index].Text);
        }

        // 如果选中索引在有效范围内，则设置选中的控制项
        if (selectedIndex >= 0 && selectedIndex < items.Count)
        {
            dialog.SetSelectedControlItem(Id, selectedIndex);
        }
        // 如果选中索引不为 -1，但不在有效范围内，则抛出异常
        else if (selectedIndex != -1)
        {
            throw new IndexOutOfRangeException(LocalizedMessages.RadioButtonListIndexOutOfBounds);
        }

        // 同步非托管属性
        SyncUnmanagedProperties();
    }
}

// 定义单选按钮项类
public class CommonFileDialogRadioButtonListItem
{
    // 默认构造函数
    public CommonFileDialogRadioButtonListItem() : this(string.Empty)
    {
    }

    // 带文本的构造函数
    public CommonFileDialogRadioButtonListItem(string text) => Text = text;

    // 单选按钮项的文本属性
    public string Text { get; set; }
}
```