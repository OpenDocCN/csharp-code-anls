# `.\better-genshin-impact\Build\MicaSetup\Natives\Shell\Dialogs\CommonFileDialogs\CommonFileDialogComboBox.cs`

```cs
# 引入必要的命名空间
﻿using System;
using System.Collections.ObjectModel;
using System.Diagnostics;
using System.Windows.Markup;

namespace MicaSetup.Shell.Dialogs;

# 指定 ContentProperty 属性为 "Items"
[ContentProperty("Items")]
# 定义 CommonFileDialogComboBox 类，继承自 CommonFileDialogProminentControl，并实现 ICommonFileDialogIndexedControls 接口
public class CommonFileDialogComboBox : CommonFileDialogProminentControl, ICommonFileDialogIndexedControls
{
    # 声明一个只读的 Collection 存储 CommonFileDialogComboBoxItem 对象
    private readonly Collection<CommonFileDialogComboBoxItem> items = new Collection<CommonFileDialogComboBoxItem>();
    # 声明一个存储选中索引的变量，初始化为 -1
    private int selectedIndex = -1;

    # 默认构造函数
    public CommonFileDialogComboBox()
    {
    }

    # 带参数的构造函数，调用基类构造函数
    public CommonFileDialogComboBox(string name)
        : base(name, string.Empty)
    {
    }

    # 选中索引变化事件
    public event EventHandler SelectedIndexChanged = delegate { };

    # 获取 items 集合
    public Collection<CommonFileDialogComboBoxItem> Items => items;

    # 选中索引属性，带有 getter 和 setter
    [System.Diagnostics.CodeAnalysis.SuppressMessage("Microsoft.Usage", "CA2201:DoNotRaiseReservedExceptionTypes")]
    public int SelectedIndex
    {
        get => selectedIndex; # 返回当前选中索引
        set
        {
            if (selectedIndex == value)
                return; # 如果新值和当前值相同，则不进行任何操作

            if (HostingDialog == null)
            {
                selectedIndex = value; # 如果 HostingDialog 为 null，则直接设置值
                return;
            }

            if (value >= 0 && value < items.Count)
            {
                selectedIndex = value; # 如果值在有效范围内，则设置值并应用属性更改
                ApplyPropertyChange("SelectedIndex");
            }
            else
            {
                throw new IndexOutOfRangeException(LocalizedMessages.ComboBoxIndexOutsideBounds); # 如果值无效，则抛出异常
            }
        }
    }

    # 实现接口方法，触发 SelectedIndexChanged 事件
    void ICommonFileDialogIndexedControls.RaiseSelectedIndexChangedEvent()
    {
        if (Enabled)
            SelectedIndexChanged(this, EventArgs.Empty);
    }

    # 重写 Attach 方法，设置控件并同步属性
    internal override void Attach(IFileDialogCustomize dialog)
    {
        Debug.Assert(dialog != null, "CommonFileDialogComboBox.Attach: dialog parameter can not be null"); # 确保 dialog 参数不为 null

        dialog!.AddComboBox(Id); # 将 ComboBox 添加到对话框

        for (var index = 0; index < items.Count; index++)
            dialog.AddControlItem(Id, index, items[index].Text); # 添加每个控件项到对话框

        if (selectedIndex >= 0 && selectedIndex < items.Count)
        {
            dialog.SetSelectedControlItem(Id, selectedIndex); # 如果选中索引有效，则设置选中项
        }
        else if (selectedIndex != -1)
        {
            throw new IndexOutOfRangeException(LocalizedMessages.ComboBoxIndexOutsideBounds); # 如果选中索引无效，则抛出异常
        }

        if (IsProminent)
            dialog.MakeProminent(Id); # 如果控件突出显示，则设置为显著

        SyncUnmanagedProperties(); # 同步非托管属性
    }
}

# 定义 CommonFileDialogComboBoxItem 类
public class CommonFileDialogComboBoxItem
{
    # 声明一个文本字段，初始化为空字符串
    private string text = string.Empty;

    # 默认构造函数
    public CommonFileDialogComboBoxItem()
    {
    }

    # 带文本参数的构造函数
    public CommonFileDialogComboBoxItem(string text) => this.text = text;

    # 文本属性的 getter 和 setter
    public string Text
    {
        get => text; # 返回文本值
        set => text = value; # 设置文本值
    }
}
```