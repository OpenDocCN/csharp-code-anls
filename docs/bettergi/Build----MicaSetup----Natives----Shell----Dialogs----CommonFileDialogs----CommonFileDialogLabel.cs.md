# `.\better-genshin-impact\Build\MicaSetup\Natives\Shell\Dialogs\CommonFileDialogs\CommonFileDialogLabel.cs`

```cs
# 引入系统诊断命名空间，用于调试相关功能
﻿using System.Diagnostics;

# 定义命名空间 MicaSetup.Shell.Dialogs
namespace MicaSetup.Shell.Dialogs;

# 定义一个名为 CommonFileDialogLabel 的类，继承自 CommonFileDialogControl
public class CommonFileDialogLabel : CommonFileDialogControl
{
    # 默认构造函数
    public CommonFileDialogLabel()
    {
    }

    # 带有文本参数的构造函数，调用基类构造函数
    public CommonFileDialogLabel(string text) : base(text)
    {
    }

    # 带有名称和文本参数的构造函数，调用基类构造函数
    public CommonFileDialogLabel(string name, string text) : base(name, text)
    {
    }

    # 覆盖基类的 Attach 方法，将自定义控件附加到文件对话框中
    internal override void Attach(IFileDialogCustomize dialog)
    {
        # 断言对话框参数不为 null，如果为 null，则抛出错误
        Debug.Assert(dialog != null, "CommonFileDialog.Attach: dialog parameter can not be null");

        # 使用对话框对象添加文本内容
        dialog!.AddText(Id, Text);

        # 同步非托管属性
        SyncUnmanagedProperties();
    }
}
```