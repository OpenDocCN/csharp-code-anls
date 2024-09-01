# `.\better-genshin-impact\Build\MicaSetup\Natives\Shell\Dialogs\Common\ShellObjectContainer.cs`

```cs
# 声明一个抽象类 ShellContainer，继承自 ShellObject，实现 IEnumerable<ShellObject> 和 IDisposable 接口
public abstract class ShellContainer : ShellObject, IEnumerable<ShellObject>, IDisposable
{
    # 声明用于桌面文件夹枚举和原生 shell 文件夹的私有字段
    private IShellFolder desktopFolderEnumeration;
    private IShellFolder nativeShellFolder;

    # 内部构造函数
    internal ShellContainer()
    {
    }

    # 内部构造函数，接受一个 IShellItem2 对象
    internal ShellContainer(IShellItem2 shellItem) : base(shellItem)
    {
    }

    # 内部属性，获取原生 shell 文件夹
    internal IShellFolder NativeShellFolder
    {
        get
        {
            # 如果 nativeShellFolder 为 null，执行以下操作
            if (nativeShellFolder == null)
            {
                # 定义 IShellFolder 的 GUID 和 ShellFolderObject 的 GUID
                var guid = new Guid(ShellIIDGuid.IShellFolder);
                var handler = new Guid(ShellBHIDGuid.ShellFolderObject);

                # 尝试将 NativeShellItem 绑定到 handler，返回 IShellFolder 对象
                var hr = NativeShellItem.BindToHandler(
                    0, ref handler, ref guid, out nativeShellFolder);

                # 如果绑定失败
                if (CoreErrorHelper.Failed(hr))
                {
                    # 获取解析名称，如果名称不等于桌面路径，抛出 ShellException
                    var str = ShellHelper.GetParsingName(NativeShellItem);
                    if (str != null && str != Environment.GetFolderPath(Environment.SpecialFolder.Desktop))
                    {
                        throw new ShellException(hr);
                    }
                }
            }

            # 返回原生 shell 文件夹
            return nativeShellFolder;
        }
    }

    # 实现 IEnumerable<ShellObject> 的 GetEnumerator 方法
    public IEnumerator<ShellObject> GetEnumerator()
    {
        # 如果 NativeShellFolder 为 null，尝试获取桌面文件夹枚举
        if (NativeShellFolder == null)
        {
            if (desktopFolderEnumeration == null)
            {
                Shell32.SHGetDesktopFolder(out desktopFolderEnumeration);
            }

            nativeShellFolder = desktopFolderEnumeration;
        }

        # 返回 ShellFolderItems 的迭代器
        return new ShellFolderItems(this);
    }

    # 实现 IEnumerable.GetEnumerator 方法
    System.Collections.IEnumerator System.Collections.IEnumerable.GetEnumerator() => new ShellFolderItems(this);

    # 重写 Dispose 方法，释放资源
    protected override void Dispose(bool disposing)
    {
        # 释放原生 shell 文件夹的 COM 对象
        if (nativeShellFolder != null)
        {
            Marshal.ReleaseComObject(nativeShellFolder);
            nativeShellFolder = null!;
        }

        # 释放桌面文件夹枚举的 COM 对象
        if (desktopFolderEnumeration != null)
        {
            Marshal.ReleaseComObject(desktopFolderEnumeration);
            desktopFolderEnumeration = null!;
        }

        # 调用基类的 Dispose 方法
        base.Dispose(disposing);
    }
}
```