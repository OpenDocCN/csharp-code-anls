# `.\better-genshin-impact\Build\MicaSetup\Natives\Shell\Dialogs\Common\ShellObjectFactory.cs`

```cs
# 定义一个静态类 ShellObjectFactory，用于创建不同类型的 ShellObject 实例
internal static class ShellObjectFactory
{
    # 根据 IShellItem 接口创建相应的 ShellObject 实例
    internal static ShellObject Create(IShellItem nativeShellItem)
    {
        # 确保传入的 nativeShellItem 不为空
        Debug.Assert(nativeShellItem != null, "nativeShellItem should not be null");

        # 如果操作系统版本低于 Windows Vista，则抛出平台不支持异常
        if (!OsVersionHelper.IsWindowsVista_OrGreater)
        {
            throw new PlatformNotSupportedException(LocalizedMessages.ShellObjectFactoryPlatformNotSupported);
        }

        # 尝试将 nativeShellItem 转换为 IShellItem2 类型
        var nativeShellItem2 = nativeShellItem as IShellItem2;

        # 获取 nativeShellItem2 的项类型
        var itemType = ShellHelper.GetItemType(nativeShellItem2!);

        # 如果 itemType 不为空，则将其转换为小写字母
        if (!string.IsNullOrEmpty(itemType)) { itemType = itemType.ToLowerInvariant(); }

        # 获取项的属性
        nativeShellItem2!.GetAttributes(ShellFileGetAttributesOptions.FileSystem | ShellFileGetAttributesOptions.Folder, out var sfgao);

        # 判断项是否是文件系统
        var isFileSystem = (sfgao & ShellFileGetAttributesOptions.FileSystem) != 0;

        # 判断项是否是文件夹
        var isFolder = (sfgao & ShellFileGetAttributesOptions.Folder) != 0;

        # 如果项类型是 ".lnk" 文件，则创建 ShellLink 对象
        if (StringComparer.OrdinalIgnoreCase.Equals(itemType, ".lnk"))
        {
            return new ShellLink(nativeShellItem2);
        }
        # 如果项是文件夹
        else if (isFolder)
        {
            ShellLibrary shellLibrary;
            # 如果项类型是 ".library-ms" 文件且能够创建 ShellLibrary 对象，则返回该对象
            if (itemType == ".library-ms" && (shellLibrary = ShellLibrary.FromShellItem(nativeShellItem2, true)) != null!)
            {
                return shellLibrary;
            }
            # 如果项类型是 ".searchconnector-ms" 文件，则创建 ShellSearchConnector 对象
            else if (itemType == ".searchconnector-ms")
            {
                return new ShellSearchConnector(nativeShellItem2);
            }
            # 如果项类型是 ".search-ms" 文件，则创建 ShellSavedSearchCollection 对象
            else if (itemType == ".search-ms")
            {
                return new ShellSavedSearchCollection(nativeShellItem2);
            }

            # 如果项是文件系统
            if (isFileSystem)
            {
                # 如果不是虚拟已知文件夹，则创建 FileSystemKnownFolder 对象
                if (!IsVirtualKnownFolder(nativeShellItem2))
                {
                    var kf = new FileSystemKnownFolder(nativeShellItem2);
                    return kf;
                }

                # 否则，创建 ShellFileSystemFolder 对象
                return new ShellFileSystemFolder(nativeShellItem2);
            }

            # 如果是虚拟已知文件夹，则创建 NonFileSystemKnownFolder 对象
            if (IsVirtualKnownFolder(nativeShellItem2))
            {
                var kf = new NonFileSystemKnownFolder(nativeShellItem2);
                return kf;
            }

            # 否则，创建 ShellNonFileSystemFolder 对象
            return new ShellNonFileSystemFolder(nativeShellItem2);
        }

        # 如果项是文件系统，则创建 ShellFile 对象
        if (isFileSystem) { return new ShellFile(nativeShellItem2); }

        # 否则，创建 ShellNonFileSystemItem 对象
        return new ShellNonFileSystemItem(nativeShellItem2);
    }

    # 根据解析名称创建相应的 ShellObject 实例
    internal static ShellObject Create(string parsingName)
    {
        // 检查传入的解析名称是否为空，如果为空则抛出异常
        if (string.IsNullOrEmpty(parsingName))
        {
            throw new ArgumentNullException("parsingName");
        }

        // 创建一个 Guid 对象，用于表示 IShellItem2 接口
        var guid = new Guid(ShellIIDGuid.IShellItem2);
        // 调用 Shell32 的方法来创建 IShellItem2 对象
        var retCode = Shell32.SHCreateItemFromParsingName(parsingName, 0, ref guid, out
        IShellItem2 nativeShellItem);

        // 检查返回代码是否表示成功，如果失败则抛出异常
        if (!CoreErrorHelper.Succeeded(retCode))
        {
            throw new ShellException(LocalizedMessages.ShellObjectFactoryUnableToCreateItem, Marshal.GetExceptionForHR(retCode));
        }
        // 返回通过 ShellObjectFactory 创建的 ShellObject 对象
        return ShellObjectFactory.Create(nativeShellItem);
    }

    internal static ShellObject Create(nint idListPtr)
    {
        // 检查操作系统版本是否为 Vista 或更高版本，如果不是则抛出异常
        OsVersionHelper.ThrowIfNotVista();

        // 创建一个 Guid 对象，用于表示 IShellItem2 接口
        var guid = new Guid(ShellIIDGuid.IShellItem2);

        // 调用 Shell32 的方法来通过 IDList 创建 IShellItem2 对象
        var retCode = Shell32.SHCreateItemFromIDList(idListPtr, ref guid, out var nativeShellItem);

        // 检查返回代码是否表示成功，如果失败则返回 null
        if (!CoreErrorHelper.Succeeded(retCode)) { return null!; }
        // 返回通过 ShellObjectFactory 创建的 ShellObject 对象
        return ShellObjectFactory.Create(nativeShellItem);
    }

    internal static ShellObject Create(nint idListPtr, ShellContainer parent)
    {
        // 调用 Shell32 的方法来创建 ShellItem 对象，并指定父容器
        var retCode = Shell32.SHCreateShellItem(
            0,
            parent.NativeShellFolder,
            idListPtr, out var nativeShellItem);

        // 检查返回代码是否表示成功，如果失败则返回 null
        if (!CoreErrorHelper.Succeeded(retCode)) { return null!; }

        // 返回通过 ShellObjectFactory 创建的 ShellObject 对象
        return ShellObjectFactory.Create(nativeShellItem);
    }

    private static bool IsVirtualKnownFolder(IShellItem2 nativeShellItem2)
    {
        // 初始化 PIDL 指针
        nint pidl = 0;
        try
        {
            // 声明本地文件夹对象和文件夹定义
            IKnownFolderNative nativeFolder = null!;
            var definition = new KnownFoldersSafeNativeMethods.NativeFolderDefinition();

            // 创建一个对象用于锁定
            var padlock = new object();
            lock (padlock)
            {
                // 获取原始 COM 对象的 IUnknown 指针
                var unknown = Marshal.GetIUnknownForObject(nativeShellItem2);

                // 在线程池中异步处理
                ThreadPool.QueueUserWorkItem(obj =>
                {
                    lock (padlock)
                    {
                        // 从 IUnknown 指针获取 PIDL
                        pidl = ShellHelper.PidlFromUnknown(unknown);

                        // 根据 PIDL 查找文件夹
                        new KnownFolderManagerClass().FindFolderFromIDList(pidl, out nativeFolder);

                        // 获取文件夹定义
                        nativeFolder?.GetFolderDefinition(out definition);

                        // 解除锁定
                        Monitor.Pulse(padlock);
                    }
                });

                // 等待线程池任务完成
                Monitor.Wait(padlock);
            }

            // 检查文件夹是否存在且是虚拟文件夹
            return nativeFolder != null && definition.category == FolderCategory.Virtual;
        }
        finally
        {
            // 释放 PIDL 资源
            Shell32.ILFree(pidl);
        }
    }
# 结束一个代码块或代码结构（可能是函数、类、条件语句等）
}
```