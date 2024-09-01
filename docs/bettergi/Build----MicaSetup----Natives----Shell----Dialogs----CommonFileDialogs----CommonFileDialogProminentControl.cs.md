# `.\better-genshin-impact\Build\MicaSetup\Natives\Shell\Dialogs\CommonFileDialogs\CommonFileDialogProminentControl.cs`

```cs
# 引入系统 Windows 标记相关命名空间
﻿using System.Windows.Markup;

# 定义 MicaSetup.Shell.Dialogs 命名空间
namespace MicaSetup.Shell.Dialogs;

# 指定此类的 ContentProperty 属性为 "Items"
[ContentProperty("Items")]
# 定义一个抽象类 CommonFileDialogProminentControl，继承自 CommonFileDialogControl
public abstract class CommonFileDialogProminentControl : CommonFileDialogControl
{
    # 私有字段，用于存储是否显著的状态
    private bool isProminent;

    # 保护构造函数，无参数
    protected CommonFileDialogProminentControl()
    {
    }

    # 保护构造函数，带有一个文本参数
    protected CommonFileDialogProminentControl(string text) : base(text)
    {
    }

    # 保护构造函数，带有名称和文本两个参数
    protected CommonFileDialogProminentControl(string name, string text) : base(name, text)
    {
    }

    # 公共属性，用于获取或设置是否显著的状态
    public bool IsProminent
    {
        get => isProminent; # 获取 isProminent 字段的值
        set => isProminent = value; # 设置 isProminent 字段的值
    }
}
```