# `.\better-genshin-impact\Build\MicaSetup\Natives\Shell\Dialogs\CommonFileDialogs\CommonFileDialogMenu.cs`

```cs
# 引入所需的命名空间
﻿using System;
using System.Collections.ObjectModel;
using System.Diagnostics;
using System.Windows.Markup;

namespace MicaSetup.Shell.Dialogs;

# 声明类 CommonFileDialogMenu，继承自 CommonFileDialogProminentControl
[ContentProperty("Items")]
public class CommonFileDialogMenu : CommonFileDialogProminentControl
{
    # 创建私有只读集合，用于存储菜单项
    private readonly Collection<CommonFileDialogMenuItem> items = new Collection<CommonFileDialogMenuItem>();

    # 默认构造函数
    public CommonFileDialogMenu() : base()
    {
    }

    # 带有文本参数的构造函数
    public CommonFileDialogMenu(string text) : base(text)
    {
    }

    # 带有名称和文本参数的构造函数
    public CommonFileDialogMenu(string name, string text) : base(name, text)
    {
    }

    # 提供对菜单项集合的公开访问
    public Collection<CommonFileDialogMenuItem> Items => items;

    # 重写 Attach 方法以绑定到 IFileDialogCustomize 对象
    internal override void Attach(IFileDialogCustomize dialog)
    {
        # 断言对话框参数不为空
        Debug.Assert(dialog != null, "CommonFileDialogMenu.Attach: dialog parameter can not be null");

        # 将菜单添加到对话框
        dialog!.AddMenu(Id, Text);

        # 遍历所有菜单项并将它们添加到对话框中
        foreach (var item in items)
            dialog.AddControlItem(Id, item.Id, item.Text);

        # 如果菜单是突出的，则使其突出显示
        if (IsProminent)
            dialog.MakeProminent(Id);

        # 同步未管理的属性
        SyncUnmanagedProperties();
    }
}

# 声明类 CommonFileDialogMenuItem，继承自 CommonFileDialogControl
public class CommonFileDialogMenuItem : CommonFileDialogControl
{
    # 默认构造函数
    public CommonFileDialogMenuItem() : base(string.Empty)
    {
    }

    # 带有文本参数的构造函数
    public CommonFileDialogMenuItem(string text) : base(text)
    {
    }

    # 声明 Click 事件，事件处理程序默认为空
    public event EventHandler Click = delegate { };

    # 重写 Attach 方法，未实现具体内容
    internal override void Attach(IFileDialogCustomize dialog)
    {
    }

    # 触发 Click 事件
    internal void RaiseClickEvent()
    {
        # 如果控件启用，则触发 Click 事件
        if (Enabled) { Click(this, EventArgs.Empty); }
    }
}
```