# `.\better-genshin-impact\Build\MicaSetup\Natives\Shell\Dialogs\Common\ShellSearchCollection.cs`

```cs
# 声明一个命名空间 MicaSetup.Shell.Dialogs
﻿namespace MicaSetup.Shell.Dialogs;

# 定义一个类 ShellSearchCollection，继承自 ShellContainer
public class ShellSearchCollection : ShellContainer
{
    # 内部构造函数，没有参数
    internal ShellSearchCollection()
    {
    }

    # 内部构造函数，接受一个 IShellItem2 对象作为参数，调用基类构造函数
    internal ShellSearchCollection(IShellItem2 shellItem) : base(shellItem)
    {
    }
}
```