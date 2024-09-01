# `.\better-genshin-impact\Build\MicaSetup\Natives\Shell\Dialogs\Interop\Common\ShellNativeMethods.cs`

```cs
# 定义了与 Windows Shell 相关的原生方法和常量
using MicaSetup.Natives;
using System;
using System.Runtime.InteropServices;

namespace MicaSetup.Shell.Dialogs;

# 壳体原生方法的静态类
internal static class ShellNativeMethods
{
    # 常量定义，用于表示命令链接
    internal const int CommandLink = 0x0000000E;

    # 常量定义，用于获取笔记
    internal const uint GetNote = 0x0000160A;

    # 常量定义，用于获取笔记长度
    internal const uint GetNoteLength = 0x0000160B;

    # 常量定义，用于表示字符串被截断
    internal const int InPlaceStringTruncated = 0x00401A0;

    # 常量定义，最大路径长度
    internal const int MaxPath = 260;

    # 常量定义，用于设置笔记
    internal const uint SetNote = 0x00001609;

    # 常量定义，用于设置保护盾
    internal const uint SetShield = 0x0000160C;
}

# 壳体32 API 的静态部分类
public static partial class Shell32
{
    # 导入 Shell32.dll 的 ILFree 方法，用于释放 ITEMIDLIST
    [DllImport(Lib.Shell32, CharSet = CharSet.None)]
    public static extern void ILFree(nint pidl);

    # 导入 Shell32.dll 的 ILGetSize 方法，用于获取 ITEMIDLIST 的大小
    [DllImport(Lib.Shell32, CharSet = CharSet.Unicode, SetLastError = true)]
    public static extern uint ILGetSize(nint pidl);

    # 导入 Shell32.dll 的 SHChangeNotification_Lock 方法，用于锁定更改通知
    [DllImport(Lib.Shell32)]
    public static extern nint SHChangeNotification_Lock(nint windowHandle, int processId, out nint pidl, out uint lEvent);

    # 导入 Shell32.dll 的 SHChangeNotification_Unlock 方法，用于解锁更改通知
    [DllImport(Lib.Shell32)]
    [return: MarshalAs(UnmanagedType.Bool)]
    public static extern bool SHChangeNotification_Unlock(nint hLock);

    # 导入 Shell32.dll 的 SHChangeNotifyDeregister 方法，用于注销更改通知
    [DllImport(Lib.Shell32)]
    [return: MarshalAs(UnmanagedType.Bool)]
    public static extern bool SHChangeNotifyDeregister(uint hNotify);

    # 导入 Shell32.dll 的 SHChangeNotifyRegister 方法，用于注册更改通知
    [DllImport(Lib.Shell32)]
    public static extern uint SHChangeNotifyRegister(
        nint windowHandle,
        ShellChangeNotifyEventSource sources,
        ShellObjectChangeTypes events,
        uint message,
        int entries,
        ref SHChangeNotifyEntry changeNotifyEntry);

    # 导入 Shell32.dll 的 SHCreateItemFromIDList 方法，用于从 ITEMIDLIST 创建 Shell 项
    [DllImport(Lib.Shell32, CharSet = CharSet.Unicode, SetLastError = true)]
    public static extern int SHCreateItemFromIDList(
        nint pidl,
        ref Guid riid,
        [MarshalAs(UnmanagedType.Interface)] out IShellItem2 ppv);

    # 导入 Shell32.dll 的 SHCreateItemFromParsingName 方法，用于从路径创建 Shell 项（IShellItem2）
    [DllImport(Lib.Shell32, CharSet = CharSet.Unicode, SetLastError = true)]
    public static extern int SHCreateItemFromParsingName(
        [MarshalAs(UnmanagedType.LPWStr)] string path,
        nint pbc,
        ref Guid riid,
        [MarshalAs(UnmanagedType.Interface)] out IShellItem2 shellItem);

    # 导入 Shell32.dll 的 SHCreateItemFromParsingName 方法，用于从路径创建 Shell 项（IShellItem）
    [DllImport(Lib.Shell32, CharSet = CharSet.Unicode, SetLastError = true)]
    public static extern int SHCreateItemFromParsingName(
        [MarshalAs(UnmanagedType.LPWStr)] string path,
        nint pbc,
        ref Guid riid,
        [MarshalAs(UnmanagedType.Interface)] out IShellItem shellItem);

    # 导入 Shell32.dll 的 SHCreateShellItem 方法，用于创建 Shell 项
    [DllImport(Lib.Shell32, CharSet = CharSet.Unicode, SetLastError = true)]
    public static extern int SHCreateShellItem(
        nint pidlParent,
        [In, MarshalAs(UnmanagedType.Interface)] IShellFolder psfParent,
        nint pidl,
        [MarshalAs(UnmanagedType.Interface)] out IShellItem ppsi
    );

    # 导入 Shell32.dll 的 SHCreateShellItemArrayFromDataObject 方法，用于从数据对象创建 Shell 项数组
    [DllImport(Lib.Shell32, CharSet = CharSet.Unicode, SetLastError = true)]
    public static extern int SHCreateShellItemArrayFromDataObject(
        System.Runtime.InteropServices.ComTypes.IDataObject pdo,
        ref Guid riid,
        [MarshalAs(UnmanagedType.Interface)] out IShellItemArray iShellItemArray);
    # 导入 Shell32 库中的 SHGetDesktopFolder 函数，用于获取桌面文件夹的 IShellFolder 接口
    [DllImport(Lib.Shell32, CharSet = CharSet.Unicode, SetLastError = true)]
    public static extern int SHGetDesktopFolder(
        # 定义一个输出参数 ppshf，它将接收桌面文件夹的 IShellFolder 接口
        [MarshalAs(UnmanagedType.Interface)] out IShellFolder ppshf
    );

    # 导入 Shell32 库中的 SHGetIDListFromObject 函数，用于从 COM 对象中获取 PIDL（项标识符列表）
    [DllImport(Lib.Shell32, CharSet = CharSet.Unicode, SetLastError = true)]
    public static extern int SHGetIDListFromObject(nint iUnknown,
        # 定义一个输出参数 ppidl，它将接收 PIDL
        out nint ppidl
    );

    # 导入 Shell32 库中的 SHParseDisplayName 函数，用于将显示名称解析为 PIDL
    [DllImport(Lib.Shell32, CharSet = CharSet.Unicode, SetLastError = true)]
    public static extern int SHParseDisplayName(
        # 定义一个输入参数 pszName，指定要解析的显示名称
        [MarshalAs(UnmanagedType.LPWStr)] string pszName,
        # 定义一个输入参数 pbc，表示 IBindCtx 接口，用于解析过程的上下文
        nint pbc,
        # 定义一个输出参数 ppidl，它将接收解析结果的 PIDL
        out nint ppidl,
        # 定义一个输入参数 sfgaoIn，指定属性标志
        ShellFileGetAttributesOptions sfgaoIn,
        # 定义一个输出参数 psfgaoOut，它将接收解析后的属性标志
        out ShellFileGetAttributesOptions psfgaoOut
    );

    # 导入 Shell32 库中的 SHShowManageLibraryUI 函数，用于显示管理库的用户界面
    [DllImport(Lib.Shell32, CharSet = CharSet.Unicode, CallingConvention = CallingConvention.Winapi, SetLastError = true)]
    public static extern int SHShowManageLibraryUI(
        # 定义一个输入参数 library，表示要管理的库的 IShellItem 接口
        [In, MarshalAs(UnmanagedType.Interface)] IShellItem library,
        # 定义一个输入参数 hwndOwner，表示拥有者窗口的句柄
        [In] nint hwndOwner,
        # 定义一个输入参数 title，表示用户界面的标题
        [In] string title,
        # 定义一个输入参数 instruction，表示用户界面的说明
        [In] string instruction,
        # 定义一个输入参数 lmdOptions，表示管理库对话框的选项
        [In] LibraryManageDialogOptions lmdOptions);
}

[StructLayout(LayoutKind.Sequential, CharSet = CharSet.Auto)]
public struct FilterSpec
{
    // 字符串的名称，使用 LPWSTR 类型进行 Marshal
    [MarshalAs(UnmanagedType.LPWStr)]
    public string Name;

    // 字符串的规格，使用 LPWSTR 类型进行 Marshal
    [MarshalAs(UnmanagedType.LPWStr)]
    public string Spec;

    // 带参数的构造函数，用于初始化 Name 和 Spec 字段
    public FilterSpec(string name, string spec)
    {
        Name = name;
        Spec = spec;
    }
}

[StructLayout(LayoutKind.Sequential)]
public struct SHChangeNotifyEntry
{
    // 用于存储 IDL 指针的字段，nint 类型代表指针大小的整数
    public nint pIdl;

    // 是否递归通知的布尔值
    [MarshalAs(UnmanagedType.Bool)]
    public bool recursively;
}

[StructLayout(LayoutKind.Sequential)]
public struct ShellNotifyStruct
{
    // 第一个通知项的指针
    public nint item1;
    // 第二个通知项的指针
    public nint item2;
};

[StructLayout(LayoutKind.Sequential, CharSet = CharSet.Auto)]
public struct ThumbnailId
{
    // 图像的缩略图 ID，使用 LPArray 类型进行 Marshal，SizeParamIndex = 16 指定数组的大小
    [MarshalAs(UnmanagedType.LPArray, SizeParamIndex = 16)]
    private readonly byte rgbKey;
}

[Flags]
public enum ShellObjectChangeTypes
{
    // 无变更
    None = 0,
    // 项重命名
    ItemRename = 0x00000001,
    // 项创建
    ItemCreate = 0x00000002,
    // 项删除
    ItemDelete = 0x00000004,
    // 目录创建
    DirectoryCreate = 0x00000008,
    // 目录删除
    DirectoryDelete = 0x00000010,
    // 媒介插入
    MediaInsert = 0x00000020,
    // 媒介移除
    MediaRemove = 0x00000040,
    // 驱动器移除
    DriveRemove = 0x00000080,
    // 驱动器添加
    DriveAdd = 0x00000100,
    // 网络共享
    NetShare = 0x00000200,
    // 网络取消共享
    NetUnshare = 0x00000400,
    // 属性更改
    AttributesChange = 0x00000800,
    // 目录内容更新
    DirectoryContentsUpdate = 0x00001000,
    // 更新
    Update = 0x00002000,
    // 服务器断开
    ServerDisconnect = 0x00004000,
    // 系统图像更新
    SystemImageUpdate = 0x00008000,
    // 目录重命名
    DirectoryRename = 0x00020000,
    // 空间释放
    FreeSpace = 0x00040000,
    // 关联更改
    AssociationChange = 0x08000000,
    // 磁盘事件掩码
    DiskEventsMask = 0x0002381F,
    // 全局事件掩码
    GlobalEventsMask = 0x0C0581E0,
    // 所有事件掩码
    AllEventsMask = 0x7FFFFFFF,
    // 从中断产生的事件
    FromInterrupt = unchecked((int)0x80000000),
}

public enum ControlState
{
    // 控件无效
    Inactive = 0x00000000,
    // 控件启用
    Enable = 0x00000001,
    // 控件可见
    Visible = 0x00000002,
}

public enum DefaultSaveFolderType
{
    // 自动检测文件夹类型
    Detect = 1,
    // 私有文件夹
    Private = 2,
    // 公共文件夹
    Public = 3,
};

public enum FileDialogAddPlacement
{
    // 添加到对话框底部
    Bottom = 0x00000000,
    // 添加到对话框顶部
    Top = 0x00000001,
}

public enum FileDialogEventOverwriteResponse
{
    // 默认响应
    Default = 0x00000000,
    // 接受覆盖
    Accept = 0x00000001,
    // 拒绝覆盖
    Refuse = 0x00000002,
}

public enum FileDialogEventShareViolationResponse
{
    // 默认响应
    Default = 0x00000000,
    // 接受共享
    Accept = 0x00000001,
    // 拒绝共享
    Refuse = 0x00000002,
}

[Flags]
public enum FileOpenOptions
{
    // 显示覆盖提示
    OverwritePrompt = 0x00000002,
    // 严格文件类型
    StrictFileTypes = 0x00000004,
    // 不改变目录
    NoChangeDirectory = 0x00000008,
    // 选择文件夹
    PickFolders = 0x00000020,
    // 强制文件系统
    ForceFilesystem = 0x00000040,
    // 所有非存储项
    AllNonStorageItems = 0x00000080,
    // 不验证
    NoValidate = 0x00000100,
    // 允许多选
    AllowMultiSelect = 0x00000200,
    // 路径必须存在
    PathMustExist = 0x00000800,
    // 文件必须存在
    FileMustExist = 0x00001000,
    // 创建提示
    CreatePrompt = 0x00002000,
    // 共享感知
    ShareAware = 0x00004000,
    // 不返回只读
    NoReadOnlyReturn = 0x00008000,
    // 不测试文件创建
    NoTestFileCreate = 0x00010000,
    // 隐藏最近使用的地方
    HideMruPlaces = 0x00020000,
    // 隐藏固定的地方
    HidePinnedPlaces = 0x00040000,
    // 不解析链接
    NoDereferenceLinks = 0x00100000,
    // 不添加到最近使用的文件
    DontAddToRecent = 0x02000000,
    // 强制显示隐藏文件
    ForceShowHidden = 0x10000000,
    // 默认不使用迷你模式
    DefaultNoMiniMode = 0x20000000,
}

[Flags]
public enum GetPropertyStoreOptions
{
    // 默认选项
    Default = 0,
    // 仅处理属性
    HandlePropertiesOnly = 0x1,
    // 读写权限
    ReadWrite = 0x2,
    // 临时存储
    Temporary = 0x4,
    # 定义一个常量，表示仅限快速属性
    FastPropertiesOnly = 0x8,
    # 定义一个常量，表示打开低优先级的项
    OpensLowItem = 0x10,
    # 定义一个常量，表示延迟创建
    DelayCreation = 0x20,
    # 定义一个常量，表示最佳努力
    BestEffort = 0x40,
    # 定义一个常量，表示掩码有效位
    MaskValid = 0xff,
}
# 结束上一个代码块的闭合括号

public enum LibraryFolderFilter
{
    # 用于强制使用文件系统
    ForceFileSystem = 1,
    # 用于存储项
    StorageItems = 2,
    # 用于所有项
    AllItems = 3,
};

public enum LibraryManageDialogOptions
{
    # 默认选项
    Default = 0,
    # 警告非可索引位置
    NonIndexableLocationWarning = 1,
};

[Flags]
public enum LibraryOptions
{
    # 默认选项
    Default = 0,
    # 固定到导航窗格
    PinnedToNavigationPane = 0x1,
    # 掩码所有选项
    MaskAll = 0x1,
};

public enum LibrarySaveOptions
{
    # 如果存在则失败
    FailIfThere = 0,
    # 覆盖现有项
    OverrideExisting = 1,
    # 创建唯一名称
    MakeUniqueName = 2,
};

[Flags]
public enum ShellChangeNotifyEventSource
{
    # 中断级别
    InterruptLevel = 0x0001,
    # Shell 级别
    ShellLevel = 0x0002,
    # 递归中断
    RecursiveInterrupt = 0x1000,
    # 新交付
    NewDelivery = 0x8000,
}

[Flags]
public enum ShellFileGetAttributesOptions
{
    # 可以复制
    CanCopy = 0x00000001,
    # 可以移动
    CanMove = 0x00000002,
    # 可以链接
    CanLink = 0x00000004,
    # 存储
    Storage = 0x00000008,
    # 可以重命名
    CanRename = 0x00000010,
    # 可以删除
    CanDelete = 0x00000020,
    # 有属性表
    HasPropertySheet = 0x00000040,
    # 放置目标
    DropTarget = 0x00000100,
    # 能力掩码
    CapabilityMask = 0x00000177,
    # 系统
    System = 0x00001000,
    # 加密
    Encrypted = 0x00002000,
    # 是慢的
    IsSlow = 0x00004000,
    # 幽灵
    Ghosted = 0x00008000,
    # 链接
    Link = 0x00010000,
    # 共享
    Share = 0x00020000,
    # 只读
    ReadOnly = 0x00040000,
    # 隐藏
    Hidden = 0x00080000,
    # 显示属性掩码
    DisplayAttributeMask = 0x000FC000,
    # 文件系统祖先
    FileSystemAncestor = 0x10000000,
    # 文件夹
    Folder = 0x20000000,
    # 文件系统
    FileSystem = 0x40000000,
    # 有子文件夹
    HasSubFolder = unchecked((int)0x80000000),
    # 内容掩码
    ContentsMask = unchecked((int)0x80000000),
    # 验证
    Validate = 0x01000000,
    # 可移动
    Removable = 0x02000000,
    # 压缩
    Compressed = 0x04000000,
    # 可浏览
    Browsable = 0x08000000,
    # 非枚举
    Nonenumerated = 0x00100000,
    # 新内容
    NewContent = 0x00200000,
    # 可以标记
    CanMoniker = 0x00400000,
    # 有存储
    HasStorage = 0x00400000,
    # 流
    Stream = 0x00400000,
    # 存储祖先
    StorageAncestor = 0x00800000,
    # 存储能力掩码
    StorageCapabilityMask = 0x70C50008,
    # Pkey 掩码
    PkeyMask = unchecked((int)0x81044000),
}

[Flags]
public enum ShellFolderEnumerationOptions : ushort
{
    # 检查子项
    CheckingForChildren = 0x0010,
    # 文件夹
    Folders = 0x0020,
    # 非文件夹
    NonFolders = 0x0040,
    # 包含隐藏项
    IncludeHidden = 0x0080,
    # 在首次下一个时初始化
    InitializeOnFirstNext = 0x0100,
    # 网络打印机搜索
    NetPrinterSearch = 0x0200,
    # 可共享
    Shareable = 0x0400,
    # 存储
    Storage = 0x0800,
    # 导航枚举
    NavigationEnum = 0x1000,
    # 快速项
    FastItems = 0x2000,
    # 扁平列表
    FlatList = 0x4000,
    # 启用异步
    EnableAsync = 0x8000,
}

public enum ShellItemAttributeOptions
{
    # 与
    And = 0x00000001,
    # 或
    Or = 0x00000002,
    # 兼容性
    AppCompat = 0x00000003,
    # 掩码
    Mask = 0x00000003,
    # 所有项
    AllItems = 0x00004000,
}

public enum ShellItemDesignNameOptions
{
    # 正常
    Normal = 0x00000000,
    # 父级相对解析
    ParentRelativeParsing = unchecked((int)0x80018001),
    # 桌面绝对解析
    DesktopAbsoluteParsing = unchecked((int)0x80028000),
    # 父级相对编辑
    ParentRelativeEditing = unchecked((int)0x80031001),
    # 桌面绝对编辑
    DesktopAbsoluteEditing = unchecked((int)0x8004c000),
    # 文件系统路径
    FileSystemPath = unchecked((int)0x80058000),
    # URL
    Url = unchecked((int)0x80068000),
    # 地址栏的父级相对
    ParentRelativeForAddressBar = unchecked((int)0x8007c001),
    # 父级相对
    ParentRelative = unchecked((int)0x80080001),
}

[Flags]
public enum SIIGBF
{
    # 适应大小
    ResizeToFit = 0x00,
    # 较大尺寸也可以
    BiggerSizeOk = 0x01,
    # 仅内存
    MemoryOnly = 0x02,
    # 仅图标
    IconOnly = 0x04,
    # 仅缩略图
    ThumbnailOnly = 0x08,
    # 仅在缓存中
    InCacheOnly = 0x10,
}

[Flags]
public enum ThumbnailCacheOptions
{
    # 默认选项
    Default = 0x00000000,
    # 定义一个常量 LowQuality，值为十六进制的 0x00000001
    LowQuality = 0x00000001,
    # 定义一个常量 Cached，值为十六进制的 0x00000002
    Cached = 0x00000002,
# 结束一个代码块
}

# 定义一个标志枚举，用于指定缩略图选项
[Flags]
public enum ThumbnailOptions
{
    # 不设置任何选项（默认）
    Extract = 0x00000000,
    # 仅从缓存中提取
    InCacheOnly = 0x00000001,
    # 快速提取
    FastExtract = 0x00000002,
    # 强制提取
    ForceExtraction = 0x00000004,
    # 慢速回收
    SlowReclaim = 0x00000008,
    # 提取但不缓存
    ExtractDoNotCache = 0x00000020
}
```