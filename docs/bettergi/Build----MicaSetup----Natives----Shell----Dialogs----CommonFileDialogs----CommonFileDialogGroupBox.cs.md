# `.\better-genshin-impact\Build\MicaSetup\Natives\Shell\Dialogs\CommonFileDialogs\CommonFileDialogGroupBox.cs`

```cs
# 引入系统集合、调试和 WPF 标记相关的命名空间
﻿using System.Collections.ObjectModel;
using System.Diagnostics;
using System.Windows.Markup;

# 声明 MicaSetup.Shell.Dialogs 命名空间
namespace MicaSetup.Shell.Dialogs;

# 禁用关于 null 引用的警告
#pragma warning disable CS8618

# 指定 ContentProperty 为 Items，标记该类是一个 WPF 控件
[ContentProperty("Items")]
# 定义 CommonFileDialogGroupBox 类，继承自 CommonFileDialogProminentControl
public class CommonFileDialogGroupBox : CommonFileDialogProminentControl
{
    # 定义存储 DialogControl 的集合
    private Collection<DialogControl> items;

    # 无参数构造函数，调用基类构造函数并初始化
    public CommonFileDialogGroupBox()
        : base(string.Empty) => Initialize();

    # 带文本参数的构造函数，调用基类构造函数并初始化
    public CommonFileDialogGroupBox(string text)
        : base(text) => Initialize();

    # 带名称和文本参数的构造函数，调用基类构造函数并初始化
    public CommonFileDialogGroupBox(string name, string text)
        : base(name, text) => Initialize();

    # 提供 Items 属性的访问，返回 items 集合
    public Collection<DialogControl> Items => items;

    # 内部方法，用于附加到 IFileDialogCustomize 对象并配置控件
    internal override void Attach(IFileDialogCustomize dialog)
    {
        # 确保 dialog 参数不为 null
        Debug.Assert(dialog != null, "CommonFileDialogGroupBox.Attach: dialog parameter can not be null");

        # 开始视觉组，使用 Id 和 Text 进行标识
        dialog!.StartVisualGroup(Id, Text);

        # 遍历 items 集合中的每一个 CommonFileDialogControl，配置并附加到对话框
        foreach (CommonFileDialogControl item in items)
        {
            item.HostingDialog = HostingDialog;
            item.Attach(dialog);
        }

        # 结束视觉组
        dialog.EndVisualGroup();

        # 如果 IsProminent 为 true，则使对话框显著
        if (IsProminent)
            dialog.MakeProminent(Id);

        # 同步未管理属性
        SyncUnmanagedProperties();
    }

    # 初始化方法，创建 items 集合
    private void Initialize() => items = new Collection<DialogControl>();
}
```