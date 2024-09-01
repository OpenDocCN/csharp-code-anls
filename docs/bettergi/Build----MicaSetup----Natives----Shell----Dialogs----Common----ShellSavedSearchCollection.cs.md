# `.\better-genshin-impact\Build\MicaSetup\Natives\Shell\Dialogs\Common\ShellSavedSearchCollection.cs`

```cs
# 引入 MicaSetup.Helper 命名空间
﻿using MicaSetup.Helper;

# 定义 MicaSetup.Shell.Dialogs 命名空间
namespace MicaSetup.Shell.Dialogs;

# 定义一个名为 ShellSavedSearchCollection 的类，继承自 ShellSearchCollection
public class ShellSavedSearchCollection : ShellSearchCollection
{
    # 定义一个内部构造函数，接受 IShellItem2 类型的参数，并调用基类构造函数
    internal ShellSavedSearchCollection(IShellItem2 shellItem)
        : base(shellItem) => OsVersionHelper.ThrowIfNotVista(); # 调用 OsVersionHelper 类的静态方法检查操作系统版本是否为 Vista 或更高
}
```