# `.\better-genshin-impact\Build\MicaSetup\Natives\Shell\Dialogs\Common\ShellSearchConnector.cs`

```cs
# 引用 MicaSetup.Helper 命名空间中的辅助功能
﻿using MicaSetup.Helper;

# 声明 MicaSetup.Shell.Dialogs 命名空间
namespace MicaSetup.Shell.Dialogs;

# 壳搜索连接器类，继承自 ShellSearchCollection
public sealed class ShellSearchConnector : ShellSearchCollection
{
    # 内部构造函数，检查操作系统是否为 Windows 7 或更高版本
    internal ShellSearchConnector() => OsVersionHelper.ThrowIfNotWin7();

    # 内部构造函数，接受 IShellItem2 对象并调用另一个构造函数
    internal ShellSearchConnector(IShellItem2 shellItem)
        : this() => nativeShellItem = shellItem;

    # 静态属性，检查当前平台是否支持
    public new static bool IsPlatformSupported =>
            OsVersionHelper.IsWindows7_OrGreater;
}
```