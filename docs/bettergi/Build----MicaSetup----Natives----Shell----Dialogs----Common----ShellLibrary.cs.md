# `.\better-genshin-impact\Build\MicaSetup\Natives\Shell\Dialogs\Common\ShellLibrary.cs`

```cs
# 引入 MicaSetup.Helper 命名空间
using MicaSetup.Helper;
# 引入其他系统所需的命名空间
using System;
using System.Collections.Generic;
using System.Diagnostics;
using System.IO;
using System.Linq;
using System.Runtime.InteropServices;
using System.Threading;

# 定义 MicaSetup.Shell.Dialogs 命名空间
namespace MicaSetup.Shell.Dialogs;

# 禁用警告 CS8601（可能的 null 引用未初始化）
#pragma warning disable CS8601
# 禁用警告 CS8618（非 nullable 字段未初始化）
#pragma warning disable CS8618
# 禁用警告 IDE0059（不必要的赋值）
#pragma warning disable IDE0059

# 定义一个密封类 ShellLibrary，继承自 ShellContainer，实现 IList<ShellFileSystemFolder> 接口
public sealed class ShellLibrary : ShellContainer, IList<ShellFileSystemFolder>
{
    # 定义一个内部常量字符串，表示文件扩展名
    internal const string FileExtension = ".library-ms";

    # 定义一个静态只读数组，包含不同库的 GUID
    private static readonly Guid[] FolderTypesGuids =
    {
        new Guid(ShellKFIDGuid.GenericLibrary),          # 通用库的 GUID
        new Guid(ShellKFIDGuid.DocumentsLibrary),        # 文档库的 GUID
        new Guid(ShellKFIDGuid.MusicLibrary),            # 音乐库的 GUID
        new Guid(ShellKFIDGuid.PicturesLibrary),         # 图片库的 GUID
        new Guid(ShellKFIDGuid.VideosLibrary)            # 视频库的 GUID
    };

    # 定义一个只读字段，表示已知文件夹
    private readonly IKnownFolder knownFolder;
    # 定义一个只读字段，表示本机 Shell 库接口
    private INativeShellLibrary nativeShellLibrary;

    # 构造函数：接收库名称和是否覆盖的标志
    public ShellLibrary(string libraryName, bool overwrite)
        : this()
    {
        # 如果库名称为空或 null，抛出参数异常
        if (string.IsNullOrEmpty(libraryName))
        {
            throw new ArgumentException(LocalizedMessages.ShellLibraryEmptyName, "libraryName");
        }

        # 设置库名称
        Name = libraryName;
        # 创建一个 GUID，用于表示库的类别
        var guid = new Guid(ShellKFIDGuid.Libraries);

        # 根据是否覆盖现有库设置标志
        var flags = overwrite ?
                LibrarySaveOptions.OverrideExisting :
                LibrarySaveOptions.FailIfThere;

        # 创建 ShellLibraryCoClass 的实例，并将其转换为 INativeShellLibrary 接口
        nativeShellLibrary = (INativeShellLibrary)new ShellLibraryCoClass();
        # 在已知文件夹中保存库，设置输出的 nativeShellItem
        nativeShellLibrary.SaveInKnownFolder(ref guid, libraryName, flags, out nativeShellItem);
    }

    # 构造函数：接收库名称、源已知文件夹和是否覆盖的标志
    public ShellLibrary(string libraryName, IKnownFolder sourceKnownFolder, bool overwrite)
        : this()
    {
        # 如果库名称为空或 null，抛出参数异常
        if (string.IsNullOrEmpty(libraryName))
        {
            throw new ArgumentException(LocalizedMessages.ShellLibraryEmptyName, "libraryName");
        }

        # 设置源已知文件夹
        knownFolder = sourceKnownFolder;

        # 设置库名称
        Name = libraryName;
        # 获取源已知文件夹的 GUID
        var guid = knownFolder.FolderId;

        # 根据是否覆盖现有库设置标志
        var flags = overwrite ?
                LibrarySaveOptions.OverrideExisting :
                LibrarySaveOptions.FailIfThere;

        # 创建 ShellLibraryCoClass 的实例，并将其转换为 INativeShellLibrary 接口
        nativeShellLibrary = (INativeShellLibrary)new ShellLibraryCoClass();
        # 在已知文件夹中保存库，设置输出的 nativeShellItem
        nativeShellLibrary.SaveInKnownFolder(ref guid, libraryName, flags, out nativeShellItem);
    }

    # 构造函数：接收库名称、文件夹路径和是否覆盖的标志
    public ShellLibrary(string libraryName, string folderPath, bool overwrite)
        : this()
    {
        # 如果 libraryName 为空或为 null，则抛出 ArgumentException 异常
        if (string.IsNullOrEmpty(libraryName))
        {
            throw new ArgumentException(LocalizedMessages.ShellLibraryEmptyName, "libraryName");
        }

        # 如果 folderPath 不存在，则抛出 DirectoryNotFoundException 异常
        if (!Directory.Exists(folderPath))
        {
            throw new DirectoryNotFoundException(LocalizedMessages.ShellLibraryFolderNotFound);
        }

        # 设置实例属性 Name 为 libraryName
        Name = libraryName;

        # 根据 overwrite 标志选择保存选项
        var flags = overwrite ?
                LibrarySaveOptions.OverrideExisting :
                LibrarySaveOptions.FailIfThere;

        # 创建一个表示 IShellItem 的 GUID 对象
        var guid = new Guid(ShellIIDGuid.IShellItem);

        # 从 folderPath 创建一个 IShellItem 对象
        Shell32.SHCreateItemFromParsingName(folderPath, 0, ref guid, out
        IShellItem shellItemIn);

        # 实例化 ShellLibraryCoClass 并将其转换为 INativeShellLibrary
        nativeShellLibrary = (INativeShellLibrary)new ShellLibraryCoClass();
        # 使用 shellItemIn 和其他参数保存 shellLibrary，并将结果赋值给 nativeShellItem
        nativeShellLibrary.Save(shellItemIn, libraryName, flags, out nativeShellItem);
    }

    # ShellLibrary 类的构造函数，检查操作系统版本是否为 Windows 7 或更高版本
    private ShellLibrary() => OsVersionHelper.ThrowIfNotWin7();

    # 带有 INativeShellLibrary 参数的构造函数
    private ShellLibrary(INativeShellLibrary nativeShellLibrary)
        : this() => this.nativeShellLibrary = nativeShellLibrary;

    # 带有 IKnownFolder 参数和只读标志的构造函数
    private ShellLibrary(IKnownFolder sourceKnownFolder, bool isReadOnly)
        : this()
    {
        # 断言 sourceKnownFolder 不为 null
        Debug.Assert(sourceKnownFolder != null);

        # 设置 knownFolder 为 sourceKnownFolder
        knownFolder = sourceKnownFolder;

        # 实例化 ShellLibraryCoClass 并将其转换为 INativeShellLibrary
        nativeShellLibrary = (INativeShellLibrary)new ShellLibraryCoClass();

        # 根据 isReadOnly 标志选择访问模式
        var flags = isReadOnly ?
                AccessModes.Read :
                AccessModes.ReadWrite;

        # 设置 base.nativeShellItem 为 sourceKnownFolder 的 NativeShellItem2
        base.nativeShellItem = ((ShellObject)sourceKnownFolder!).NativeShellItem2;

        # 获取 sourceKnownFolder 的 FolderId
        var guid = sourceKnownFolder.FolderId;

        try
        {
            # 从已知文件夹加载库
            nativeShellLibrary.LoadLibraryFromKnownFolder(ref guid, flags);
        }
        catch (InvalidCastException)
        {
            # 捕获 InvalidCastException 异常并抛出 ArgumentException
            throw new ArgumentException(LocalizedMessages.ShellLibraryInvalidLibrary, "sourceKnownFolder");
        }
        catch (NotImplementedException)
        {
            # 捕获 NotImplementedException 异常并抛出 ArgumentException
            throw new ArgumentException(LocalizedMessages.ShellLibraryInvalidLibrary, "sourceKnownFolder");
        }
    }

    # 析构函数，调用 Dispose(false) 方法
    ~ShellLibrary()
    {
        Dispose(false);
    }

    # 静态属性，检查是否支持当前平台
    public new static bool IsPlatformSupported =>
            OsVersionHelper.IsWindows7_OrGreater;

    # 静态属性，获取 LibrariesKnownFolder
    public static IKnownFolder LibrariesKnownFolder
    {
        get
        {
            # 检查操作系统版本是否为 Windows 7 或更高版本
            OsVersionHelper.ThrowIfNotWin7();
            # 返回 Libraries 的已知文件夹
            return KnownFolderHelper.FromKnownFolderId(new Guid(ShellKFIDGuid.Libraries));
        }
    }

    # 属性 DefaultSaveFolder 的声明
    {
        get
        {
            // 创建一个 Guid 对象，表示 Shell 项的唯一标识符
            var guid = new Guid(ShellIIDGuid.IShellItem);
    
            // 获取默认的保存文件夹，使用检测模式，输出结果到 saveFolderItem
            nativeShellLibrary.GetDefaultSaveFolder(
                DefaultSaveFolderType.Detect,
                ref guid,
                out var saveFolderItem);
    
            // 获取保存文件夹的解析名称并返回
            return ShellHelper.GetParsingName(saveFolderItem);
        }
        set
        {
            // 检查传入的路径是否为空
            if (string.IsNullOrEmpty(value))
            {
                throw new ArgumentNullException("value");
            }
    
            // 检查指定的目录是否存在
            if (!Directory.Exists(value))
            {
                throw new DirectoryNotFoundException(LocalizedMessages.ShellLibraryDefaultSaveFolderNotFound);
            }
    
            // 获取目录的完整路径
            var fullPath = new DirectoryInfo(value).FullName;
    
            // 创建一个 Guid 对象，表示 Shell 项的唯一标识符
            var guid = new Guid(ShellIIDGuid.IShellItem);
    
            // 从解析名称创建一个 Shell 项
            Shell32.SHCreateItemFromParsingName(fullPath, 0, ref guid, out IShellItem saveFolderItem);
    
            // 设置默认的保存文件夹
            nativeShellLibrary.SetDefaultSaveFolder(
                DefaultSaveFolderType.Detect,
                saveFolderItem);
    
            // 提交更改
            nativeShellLibrary.Commit();
        }
    }
    
    public IconReference IconResourceId
    {
        get
        {
            // 获取图标引用
            nativeShellLibrary.GetIcon(out var iconRef);
            // 返回新的 IconReference 对象
            return new IconReference(iconRef);
        }
    
        set
        {
            // 设置图标引用
            nativeShellLibrary.SetIcon(value.ReferencePath);
            // 提交更改
            nativeShellLibrary.Commit();
        }
    }
    
    public bool IsPinnedToNavigationPane
    {
        get
        {
            // 获取库的选项
            nativeShellLibrary.GetOptions(out LibraryOptions flags);
    
            // 检查库选项是否包含“固定到导航窗格”标志
            return (
                (flags & LibraryOptions.PinnedToNavigationPane) ==
                LibraryOptions.PinnedToNavigationPane);
        }
        set
        {
            // 初始化选项为默认值
            var flags = LibraryOptions.Default;
    
            // 根据设置值修改选项
            if (value)
            {
                flags |= LibraryOptions.PinnedToNavigationPane;
            }
            else
            {
                flags &= ~LibraryOptions.PinnedToNavigationPane;
            }
    
            // 设置库选项并提交更改
            nativeShellLibrary.SetOptions(LibraryOptions.PinnedToNavigationPane, flags);
            nativeShellLibrary.Commit();
        }
    }
    
    public bool IsReadOnly => false;
    
    public LibraryFolderType LibraryType
    {
        get
        {
            // 获取文件夹类型的 GUID
            nativeShellLibrary.GetFolderType(out var folderTypeGuid);
    
            // 从 GUID 获取文件夹类型
            return GetFolderTypefromGuid(folderTypeGuid);
        }
    
        set
        {
            // 获取指定库类型的 GUID
            var guid = FolderTypesGuids[(int)value];
            // 设置文件夹类型
            nativeShellLibrary.SetFolderType(ref guid);
            // 提交更改
            nativeShellLibrary.Commit();
        }
    }
    
    public Guid LibraryTypeId
    {
        get
        {
            // 获取文件夹类型的 GUID
            nativeShellLibrary.GetFolderType(out var folderTypeGuid);
    
            // 返回文件夹类型的 GUID
            return folderTypeGuid;
        }
    }
    
    public override string Name
    {
        get
        {
            # 如果 base.Name 为空且 NativeShellItem 不为空，则从 NativeShellItem 获取文件名并赋值给 base.Name
            if (base.Name == null && NativeShellItem != null)
            {
                base.Name = System.IO.Path.GetFileNameWithoutExtension(ShellHelper.GetParsingName(NativeShellItem));
            }

            # 返回 base.Name 的值
            return base.Name!;
        }
    }

    # 返回 ItemsList 的元素数量
    public int Count => ItemsList.Count;

    # 重写 NativeShellItem 属性，返回 NativeShellItem2
    internal override IShellItem NativeShellItem => NativeShellItem2;

    # 重写 NativeShellItem2 属性，返回 nativeShellItem
    internal override IShellItem2 NativeShellItem2 => nativeShellItem;

    # 返回由 GetFolders() 方法获取的文件夹列表
    private List<ShellFileSystemFolder> ItemsList => GetFolders();

    # 索引器，允许通过索引访问 ItemsList 中的元素
    public ShellFileSystemFolder this[int index]
    {
        get => ItemsList[index];
        set =>
            # 抛出未实现异常，因为设置索引器的值未被实现
            throw new NotImplementedException();
    }

    # 静态方法 Load，加载指定名称的 ShellLibrary
    public static ShellLibrary Load(string libraryName, bool isReadOnly)
    {
        # 检查操作系统版本，确保是 Windows 7 及以上
        OsVersionHelper.ThrowIfNotWin7();

        # 获取库文件夹路径
        var kf = KnownFolders.Libraries;
        var librariesFolderPath = (kf != null) ? kf.Path : string.Empty;

        # 创建 GUID 并构建 shellItemPath
        var guid = new Guid(ShellIIDGuid.IShellItem);
        var shellItemPath = System.IO.Path.Combine(librariesFolderPath, libraryName + FileExtension);

        # 创建 IShellItem 实例
        var hr = Shell32.SHCreateItemFromParsingName(shellItemPath, 0, ref guid, out IShellItem nativeShellItem);

        # 如果失败，则抛出 ShellException
        if (!CoreErrorHelper.Succeeded(hr))
            throw new ShellException(hr);

        # 创建 ShellLibraryCoClass 的新实例，并加载库
        var nativeShellLibrary = (INativeShellLibrary)new ShellLibraryCoClass();
        var flags = isReadOnly ?
                AccessModes.Read :
                AccessModes.ReadWrite;
        nativeShellLibrary.LoadLibraryFromItem(nativeShellItem, flags);

        # 创建 ShellLibrary 实例并进行初始化
        var library = new ShellLibrary(nativeShellLibrary);
        try
        {
            library.nativeShellItem = (IShellItem2)nativeShellItem;
            library.Name = libraryName;

            # 返回初始化好的 ShellLibrary 实例
            return library;
        }
        catch
        {
            # 捕获异常并释放资源
            library.Dispose();
            throw;
        }
    }

    # 静态方法 Load，加载指定路径的 ShellLibrary
    public static ShellLibrary Load(string libraryName, string folderPath, bool isReadOnly)
    {
        # 检查操作系统版本，确保是 Windows 7 及以上
        OsVersionHelper.ThrowIfNotWin7();

        # 构建 shellItemPath
        var shellItemPath = System.IO.Path.Combine(folderPath, libraryName + FileExtension);

        # 从文件路径创建 ShellFile 实例并获取 NativeShellItem
        var item = ShellFile.FromFilePath(shellItemPath);
        var nativeShellItem = item.NativeShellItem;

        # 创建 ShellLibraryCoClass 的新实例，并加载库
        var nativeShellLibrary = (INativeShellLibrary)new ShellLibraryCoClass();
        var flags = isReadOnly ?
                AccessModes.Read :
                AccessModes.ReadWrite;
        nativeShellLibrary.LoadLibraryFromItem(nativeShellItem, flags);

        # 创建 ShellLibrary 实例并进行初始化
        var library = new ShellLibrary(nativeShellLibrary);
        try
        {
            library.nativeShellItem = (IShellItem2)nativeShellItem;
            library.Name = libraryName;

            # 返回初始化好的 ShellLibrary 实例
            return library;
        }
        catch
        {
            # 捕获异常并释放资源
            library.Dispose();
            throw;
        }
    }

    # 静态方法 Load，加载指定已知文件夹的 ShellLibrary
    public static ShellLibrary Load(IKnownFolder sourceKnownFolder, bool isReadOnly)
    {
        # 检查操作系统版本，确保是 Windows 7 及以上
        OsVersionHelper.ThrowIfNotWin7();
        # 返回使用指定已知文件夹和只读标志的 ShellLibrary 实例
        return new ShellLibrary(sourceKnownFolder, isReadOnly);
    }
    }

    // 显示管理库用户界面，使用库名称、文件夹路径和其他参数
    public static void ShowManageLibraryUI(string libraryName, string folderPath, nint windowHandle, string title, string instruction, bool allowAllLocations)
    {
        // 加载指定名称和路径的 ShellLibrary 实例
        using ShellLibrary shellLibrary = ShellLibrary.Load(libraryName, folderPath, true);
        // 调用重载方法显示管理库 UI
        ShowManageLibraryUI(shellLibrary, windowHandle, title, instruction, allowAllLocations);
    }

    // 显示管理库用户界面，使用库名称和其他参数
    public static void ShowManageLibraryUI(string libraryName, nint windowHandle, string title, string instruction, bool allowAllLocations)
    {
        // 加载指定名称的 ShellLibrary 实例
        using ShellLibrary shellLibrary = ShellLibrary.Load(libraryName, true);
        // 调用重载方法显示管理库 UI
        ShowManageLibraryUI(shellLibrary, windowHandle, title, instruction, allowAllLocations);
    }

    // 显示管理库用户界面，使用已知文件夹源和其他参数
    public static void ShowManageLibraryUI(IKnownFolder sourceKnownFolder, nint windowHandle, string title, string instruction, bool allowAllLocations)
    {
        // 从已知文件夹源加载 ShellLibrary 实例
        using ShellLibrary shellLibrary = Load(sourceKnownFolder, true);
        // 调用重载方法显示管理库 UI
        ShowManageLibraryUI(shellLibrary, windowHandle, title, instruction, allowAllLocations);
    }

    // 添加文件系统文件夹到库中
    public void Add(ShellFileSystemFolder item)
    {
        // 如果传入的项目为空，抛出参数为空异常
        if (item == null!) { throw new ArgumentNullException("item"); }

        // 将文件夹添加到库中并提交更改
        nativeShellLibrary.AddFolder(item.NativeShellItem);
        nativeShellLibrary.Commit();
    }

    // 添加指定路径的文件夹到库中
    public void Add(string folderPath)
    {
        // 如果文件夹路径不存在，抛出目录未找到异常
        if (!Directory.Exists(folderPath))
        {
            throw new DirectoryNotFoundException(LocalizedMessages.ShellLibraryFolderNotFound);
        }

        // 从路径创建文件系统文件夹并添加到库中
        Add(ShellFileSystemFolder.FromFolderPath(folderPath));
    }

    // 清除库中的所有文件夹
    public void Clear()
    {
        // 获取库中的所有项目
        var list = ItemsList;
        // 遍历项目并从库中移除每个文件夹
        foreach (var folder in list)
        {
            nativeShellLibrary.RemoveFolder(folder.NativeShellItem);
        }

        // 提交更改
        nativeShellLibrary.Commit();
    }

    // 关闭库并释放资源
    public void Close() => Dispose();

    // 检查库中是否包含指定路径的文件夹
    public bool Contains(string fullPath)
    {
        // 如果路径为空或为 null，抛出参数为空异常
        if (string.IsNullOrEmpty(fullPath))
        {
            throw new ArgumentNullException("fullPath");
        }

        // 检查是否有文件夹与指定路径匹配
        return ItemsList.Any(folder => string.Equals(fullPath, folder.Path, StringComparison.OrdinalIgnoreCase));
    }

    // 检查库中是否包含指定文件系统文件夹
    public bool Contains(ShellFileSystemFolder item)
    {
        // 如果传入的项目为空，抛出参数为空异常
        if (item == null!)
        {
            throw new ArgumentNullException("item");
        }

        // 检查是否有文件夹与传入项的路径匹配
        return ItemsList.Any(folder => string.Equals(item.Path, folder.Path, StringComparison.OrdinalIgnoreCase));
    }

    // 获取库中所有文件系统文件夹的枚举器
    public new IEnumerator<ShellFileSystemFolder> GetEnumerator() => ItemsList.GetEnumerator();

    // 获取指定文件系统文件夹在库中的索引
    public int IndexOf(ShellFileSystemFolder item) => ItemsList.IndexOf(item);

    // 从库中移除指定的文件系统文件夹
    public bool Remove(ShellFileSystemFolder item)
    {
        // 如果传入的项目为空，抛出参数为空异常
        if (item == null!) { throw new ArgumentNullException("item"); }

        try
        {
            // 尝试从库中移除文件夹并提交更改
            nativeShellLibrary.RemoveFolder(item.NativeShellItem);
            nativeShellLibrary.Commit();
        }
        catch (COMException)
        {
            // 如果发生 COM 异常，返回 false
            return false;
        }

        // 成功移除文件夹，返回 true
        return true;
    }

    // 从库中移除指定路径的文件夹
    # 从给定的文件夹路径创建一个 ShellFileSystemFolder 对象
    var item = ShellFileSystemFolder.FromFolderPath(folderPath);
    # 调用 Remove 方法删除该文件夹并返回结果
    return Remove(item);

    # 将集合中的元素复制到指定数组中，未实现该方法，抛出 NotImplementedException 异常
    void ICollection<ShellFileSystemFolder>.CopyTo(ShellFileSystemFolder[] array, int arrayIndex) => throw new NotImplementedException();

    # 返回集合中元素的枚举器
    System.Collections.IEnumerator System.Collections.IEnumerable.GetEnumerator() => ItemsList.GetEnumerator();

    # 在指定索引插入元素，未实现该方法，抛出 NotImplementedException 异常
    void IList<ShellFileSystemFolder>.Insert(int index, ShellFileSystemFolder item) =>
        throw new NotImplementedException();

    # 移除指定索引的元素，未实现该方法，抛出 NotImplementedException 异常
    void IList<ShellFileSystemFolder>.RemoveAt(int index) =>
        throw new NotImplementedException();

    # 从原生的 ShellItem 创建一个 ShellLibrary 实例
    internal static ShellLibrary FromShellItem(IShellItem nativeShellItem, bool isReadOnly)
    {
        # 确保操作系统版本为 Windows 7 或更高版本，否则抛出异常
        OsVersionHelper.ThrowIfNotWin7();

        # 创建一个新的 ShellLibraryCoClass 对象并强制转换为 INativeShellLibrary
        var nativeShellLibrary = (INativeShellLibrary)new ShellLibraryCoClass();

        # 根据 isReadOnly 参数设置访问模式
        var flags = isReadOnly ?
                AccessModes.Read :
                AccessModes.ReadWrite;

        # 从原生 ShellItem 加载库
        nativeShellLibrary.LoadLibraryFromItem(nativeShellItem, flags);

        # 创建 ShellLibrary 实例并设置 nativeShellItem 属性
        ShellLibrary library = new(nativeShellLibrary)
        {
            nativeShellItem = (IShellItem2)nativeShellItem
        };

        # 返回创建的 ShellLibrary 实例
        return library;
    }

    # 释放资源并处理相关的 COM 对象
    protected override void Dispose(bool disposing)
    {
        # 如果 nativeShellLibrary 不为空，释放其 COM 对象并将其置为空
        if (nativeShellLibrary != null)
        {
            Marshal.ReleaseComObject(nativeShellLibrary);
            nativeShellLibrary = null!;
        }

        # 调用基类的 Dispose 方法
        base.Dispose(disposing);
    }

    # 根据给定的 Guid 获取文件夹类型
    private static LibraryFolderType GetFolderTypefromGuid(Guid folderTypeGuid)
    {
        # 遍历 FolderTypesGuids 数组，查找匹配的 Guid
        for (var i = 0; i < FolderTypesGuids.Length; i++)
        {
            if (folderTypeGuid.Equals(FolderTypesGuids[i]))
            {
                # 返回匹配的 LibraryFolderType 枚举值
                return (LibraryFolderType)i;
            }
        }
        # 如果没有找到匹配的 Guid，抛出 ArgumentOutOfRangeException 异常
        throw new ArgumentOutOfRangeException("folderTypeGuid", LocalizedMessages.ShellLibraryInvalidFolderType);
    }

    # 显示管理库 UI 窗口
    private static void ShowManageLibraryUI(ShellLibrary shellLibrary, nint windowHandle, string title, string instruction, bool allowAllLocations)
    {
        var hr = 0;

        # 创建并启动一个 STA 线程以显示管理库 UI 窗口
        var staWorker = new Thread(() =>
        {
            hr = Shell32.SHShowManageLibraryUI(
                shellLibrary.NativeShellItem,
                windowHandle,
                title,
                instruction,
                allowAllLocations ?
                   LibraryManageDialogOptions.NonIndexableLocationWarning :
                   LibraryManageDialogOptions.Default);
        });

        # 设置线程的单线程单元 (STA) 状态
        staWorker.SetApartmentState(ApartmentState.STA);
        # 启动线程
        staWorker.Start();
        # 等待线程完成
        staWorker.Join();

        # 如果函数返回错误码，抛出 ShellException 异常
        if (!CoreErrorHelper.Succeeded(hr)) { throw new ShellException(hr); }
    }

    # 获取文件夹列表
    private List<ShellFileSystemFolder> GetFolders()
    # 创建一个新的 ShellFileSystemFolder 对象的列表
    var list = new List<ShellFileSystemFolder>();

    # 定义用于获取 IShellItemArray 的 GUID
    var shellItemArrayGuid = new Guid(ShellIIDGuid.IShellItemArray);

    # 调用原生 Shell 库方法获取文件夹项数组
    var hr = nativeShellLibrary.GetFolders(LibraryFolderFilter.AllItems, ref shellItemArrayGuid, out var itemArray);

    # 检查方法调用是否成功，如果不成功则返回空列表
    if (!CoreErrorHelper.Succeeded(hr)) { return list; }

    # 获取项数组中包含的项数
    itemArray.GetCount(out var count);

    # 遍历每个项，创建 ShellFileSystemFolder 对象，并添加到列表中
    for (uint i = 0; i < count; ++i)
    {
        itemArray.GetItemAt(i, out var shellItem);
        list.Add(new ShellFileSystemFolder((shellItem as IShellItem2)!));
    }

    # 如果项数组不为空，释放 COM 对象并将其置为 null
    if (itemArray != null)
    {
        Marshal.ReleaseComObject(itemArray);
        itemArray = null;
    }

    # 返回包含所有文件夹项的列表
    return list;
你希望对哪种类型的代码进行注释？
```