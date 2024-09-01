# `.\better-genshin-impact\Build\MicaSetup\Natives\Shell\Dialogs\CommonFileDialogs\CommonFileDialogSeperator.cs`

```cs
# 引入系统诊断工具包，用于调试
﻿using System.Diagnostics;

# 定义一个命名空间 MicaSetup.Shell.Dialogs
namespace MicaSetup.Shell.Dialogs;

# 定义一个继承自 CommonFileDialogControl 的类 CommonFileDialogSeparator
public class CommonFileDialogSeparator : CommonFileDialogControl
{
    # 重写基类中的 Attach 方法，该方法是内部可见的
    internal override void Attach(IFileDialogCustomize dialog)
    {
        # 确保 dialog 参数不为空，若为空则触发断言失败
        Debug.Assert(dialog != null, "CommonFileDialogSeparator.Attach: dialog parameter can not be null");

        # 使用 dialog 对象添加一个分隔符，分隔符的标识符为 Id
        dialog!.AddSeparator(Id);

        # 同步未托管的属性到其对应的管理属性
        SyncUnmanagedProperties();
    }
}
```