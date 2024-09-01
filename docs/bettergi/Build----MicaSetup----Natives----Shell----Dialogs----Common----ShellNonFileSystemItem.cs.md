# `.\better-genshin-impact\Build\MicaSetup\Natives\Shell\Dialogs\Common\ShellNonFileSystemItem.cs`

```cs
# 定义一个名为 ShellNonFileSystemItem 的类，属于 MicaSetup.Shell.Dialogs 命名空间
public class ShellNonFileSystemItem : ShellObject
{
    # 定义一个内部构造函数，接受一个 IShellItem2 类型的参数，并将其赋值给 nativeShellItem 字段
    internal ShellNonFileSystemItem(IShellItem2 shellItem) => nativeShellItem = shellItem;
}
```