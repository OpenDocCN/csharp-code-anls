# `.\better-genshin-impact\Build\MicaSetup\Natives\Shell\Dialogs\Interop\ExplorerBrowser\ExplorerBrowserCOMInterfaces.cs`

```cs
# 引入 MicaSetup.Natives 命名空间
﻿using MicaSetup.Natives;
# 引入 System 命名空间
using System;
# 引入运行时编译器服务
using System.Runtime.CompilerServices;
# 引入运行时互操作服务
using System.Runtime.InteropServices;
# 引入字符串处理相关功能
using System.Text;

# 禁用 CS0108 警告（成员隐藏）
#pragma warning disable CS0108

# 定义 CommDlgBrowser2ViewFlags 枚举，用于表示文件对话框的不同视图标志
namespace MicaSetup.Shell.Dialogs;

# CommDlgBrowser2ViewFlags 枚举定义了文件对话框的各种视图标志
internal enum CommDlgBrowser2ViewFlags
{
    ShowAllFiles = 0x00000001,    # 显示所有文件
    IsFileSave = 0x00000002,      # 这是一个保存文件对话框
    AllowPreviewPane = 0x00000004, # 允许预览窗格
    NoSelectVerb = 0x00000008,    # 不选择动词
    NoIncludeItem = 0x00000010,   # 不包含项目
    IsFolderPicker = 0x00000020   # 这是一个文件夹选择对话框
}

# CommDlgBrowserNotifyType 枚举定义了通知类型
internal enum CommDlgBrowserNotifyType
{
    Done = 1,  # 完成通知
    Start = 2  # 开始通知
}

# CommDlgBrowserStateChange 枚举定义了浏览器状态变化类型
internal enum CommDlgBrowserStateChange
{
    SetFocus = 0,      # 设置焦点
    KillFocus = 1,     # 失去焦点
    SelectionChange = 2, # 选择变化
    Rename = 3,        # 重命名
    StateChange = 4    # 状态变化
}

# ExplorerBrowserOptions 枚举定义了资源管理器浏览器的各种选项
[Flags]
internal enum ExplorerBrowserOptions
{
    NavigateOnce = 0x00000001,       # 仅导航一次
    ShowFrames = 0x00000002,         # 显示框架
    AlwaysNavigate = 0x00000004,     # 始终导航
    NoTravelLog = 0x00000008,        # 不显示旅行日志
    NoWrapperWindow = 0x00000010,    # 不使用包装窗口
    HtmlSharepointView = 0x00000020, # HTML Sharepoint 视图
    NoBorder = 0x00000040,           # 无边框
    NoPersistViewState = 0x00000080, # 不持久化视图状态
}

# ExplorerPaneState 枚举定义了资源管理器窗格的状态
internal enum ExplorerPaneState
{
    DoNotCare = 0x00000000,   # 不关心状态
    DefaultOn = 0x00000001,  # 默认开启
    DefaultOff = 0x00000002, # 默认关闭
    StateMask = 0x0000ffff,  # 状态掩码
    InitialState = 0x00010000, # 初始状态
    Force = 0x00020000       # 强制
}

# FolderOptions 枚举定义了文件夹的各种选项
[Flags]
internal enum FolderOptions
{
    AutoArrange = 0x00000001,     # 自动排列
    AbbreviatedNames = 0x00000002, # 缩写名称
    SnapToGrid = 0x00000004,      # 对齐到网格
    OwnerData = 0x00000008,       # 拥有者数据
    BestFitWindow = 0x00000010,   # 最佳适应窗口
    Desktop = 0x00000020,         # 桌面
    SingleSelection = 0x00000040, # 单选
    NoSubfolders = 0x00000080,    # 不显示子文件夹
    Transparent = 0x00000100,     # 透明
    NoClientEdge = 0x00000200,    # 无客户端边缘
    NoScroll = 0x00000400,        # 不滚动
    AlignLeft = 0x00000800,       # 左对齐
    NoIcons = 0x00001000,         # 无图标
    ShowSelectionAlways = 0x00002000, # 始终显示选择
    NoVisible = 0x00004000,       # 不可见
    SingleClickActivate = 0x00008000, # 单击激活
    NoWebView = 0x00010000,       # 无网页视图
    HideFilenames = 0x00020000,   # 隐藏文件名
    CheckSelect = 0x00040000,     # 检查选择
    NoEnumRefresh = 0x00080000,   # 不刷新枚举
    NoGrouping = 0x00100000,      # 不分组
    FullRowSelect = 0x00200000,   # 全行选择
    NoFilters = 0x00400000,       # 无过滤器
    NoColumnHeaders = 0x00800000, # 无列标题
    NoHeaderInAllViews = 0x01000000, # 所有视图中无标题
    ExtendedTiles = 0x02000000,   # 扩展磁贴
    TriCheckSelect = 0x04000000,  # 三状态选择
    AutoCheckSelect = 0x08000000, # 自动检查选择
    NoBrowserViewState = 0x10000000, # 不保存浏览器视图状态
    SubsetGroups = 0x20000000,    # 子集组
    UseSearchFolders = 0x40000000, # 使用搜索文件夹
    AllowRightToLeftReading = unchecked((int)0x80000000) # 允许从右到左阅读
}

# FolderViewMode 枚举定义了文件夹视图模式
internal enum FolderViewMode
{
    Auto = -1,     # 自动模式
    First = 1,     # 第一个视图模式
    Icon = 1,      # 图标视图
    SmallIcon = 2, # 小图标视图
    List = 3,      # 列表视图
    Details = 4,   # 详细信息视图
    Thumbnail = 5, # 缩略图视图
    Tile = 6,      # 磁贴视图
    Thumbstrip = 7,# 缩略图条
    Content = 8,   # 内容视图
    Last = 8       # 最后一个视图模式
}

# ShellViewGetItemObject 枚举定义了获取项对象的标志
internal enum ShellViewGetItemObject
{
    Background = 0x00000000, # 背景对象
    Selection = 0x00000001,  # 选择对象
    AllView = 0x00000002,    # 所有视图对象
    Checked = 0x00000003,    # 已检查对象
    TypeMask = 0x0000000F,   # 类型掩码
    ViewOrderFlag = unchecked((int)0x80000000) # 视图顺序标志
}

# ICommDlgBrowser3 接口定义了文件对话框浏览器的功能
[ComImport,
 Guid(ExplorerBrowserIIDGuid.ICommDlgBrowser3),
 InterfaceType(ComInterfaceType.InterfaceIsIUnknown)]
internal interface ICommDlgBrowser3
{
    # 处理默认命令的方法
    [PreserveSig]
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    HResult OnDefaultCommand(nint ppshv);

    # 处理默认命令的方法
    [PreserveSig]
    # 声明一个内部调用的方法，用于处理状态变化事件
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    HResult OnStateChange(
        nint ppshv,
        CommDlgBrowserStateChange uChange);

    # 声明一个内部调用的方法，用于包含对象到当前视图
    [PreserveSig]
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    HResult IncludeObject(
        nint ppshv,
        nint pidl);

    # 声明一个内部调用的方法，用于获取默认菜单文本
    [PreserveSig]
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    HResult GetDefaultMenuText(
        IShellView shellView,
        nint buffer,
        int bufferMaxLength);

    # 声明一个内部调用的方法，用于获取视图标志
    [PreserveSig]
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    HResult GetViewFlags(
        [Out] out uint pdwFlags);

    # 声明一个内部调用的方法，用于通知事件
    [PreserveSig]
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    HResult Notify(
        nint pshv, CommDlgBrowserNotifyType notifyType);

    # 声明一个内部调用的方法，用于获取当前过滤器
    [PreserveSig]
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    HResult GetCurrentFilter(
        StringBuilder pszFileSpec,
        int cchFileSpec);

    # 声明一个内部调用的方法，用于处理列点击事件
    [PreserveSig]
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    HResult OnColumnClicked(
        IShellView ppshv,
        int iColumn);

    # 声明一个内部调用的方法，用于处理视图预览创建事件
    [PreserveSig]
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    HResult OnPreViewCreated(IShellView ppshv);
}
# 关闭接口定义的结束标志

[ComImport,
 # 标记该接口为 COM 组件导入
 InterfaceType(ComInterfaceType.InterfaceIsIUnknown),
 # 指定 COM 接口类型为 IUnknown 接口
 Guid(ExplorerBrowserIIDGuid.IExplorerBrowser)]
 # 指定该接口的唯一标识符
internal interface IExplorerBrowser
{
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    # 指定方法实现由运行时提供
    void Initialize(nint hwndParent, [In] ref RECT prc, [In] FolderSettings pfs);
    # 初始化浏览器对象，设置父窗口句柄、窗口区域和文件夹设置

    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    # 指定方法实现由运行时提供
    void Destroy();
    # 销毁浏览器对象

    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    # 指定方法实现由运行时提供
    void SetRect([In, Out] ref nint phdwp, RECT rcBrowser);
    # 设置浏览器窗口的矩形区域

    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    # 指定方法实现由运行时提供
    void SetPropertyBag([MarshalAs(UnmanagedType.LPWStr)] string pszPropertyBag);
    # 设置属性包，通常用于传递配置信息

    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    # 指定方法实现由运行时提供
    void SetEmptyText([MarshalAs(UnmanagedType.LPWStr)] string pszEmptyText);
    # 设置当浏览器没有内容时显示的空文本

    [PreserveSig]
    # 保留返回值签名
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    # 指定方法实现由运行时提供
    HResult SetFolderSettings(FolderSettings pfs);
    # 设置文件夹的显示设置，并返回操作结果

    [PreserveSig]
    # 保留返回值签名
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    # 指定方法实现由运行时提供
    HResult Advise(nint psbe, out uint pdwCookie);
    # 订阅浏览器事件，并返回一个事件标识符

    [PreserveSig]
    # 保留返回值签名
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    # 指定方法实现由运行时提供
    HResult Unadvise([In] uint dwCookie);
    # 取消订阅浏览器事件

    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    # 指定方法实现由运行时提供
    void SetOptions([In] ExplorerBrowserOptions dwFlag);
    # 设置浏览器的选项标志

    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    # 指定方法实现由运行时提供
    void GetOptions(out ExplorerBrowserOptions pdwFlag);
    # 获取当前浏览器的选项标志

    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    # 指定方法实现由运行时提供
    void BrowseToIDList(nint pidl, uint uFlags);
    # 浏览到指定的 ID 列表

    [PreserveSig]
    # 保留返回值签名
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    # 指定方法实现由运行时提供
    HResult BrowseToObject([MarshalAs(UnmanagedType.IUnknown)] object punk, uint uFlags);
    # 浏览到指定的对象，并使用指定的标志

    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    # 指定方法实现由运行时提供
    void FillFromObject([MarshalAs(UnmanagedType.IUnknown)] object punk, int dwFlags);
    # 从指定的对象填充浏览器的内容

    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    # 指定方法实现由运行时提供
    void RemoveAll();
    # 移除浏览器中的所有项目

    [PreserveSig]
    # 保留返回值签名
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    # 指定方法实现由运行时提供
    HResult GetCurrentView(ref Guid riid, out nint ppv);
    # 获取当前视图的接口

}

[ComImport,
 # 标记该接口为 COM 组件导入
 Guid(ExplorerBrowserIIDGuid.IExplorerBrowserEvents),
 # 指定该接口的唯一标识符
 InterfaceType(ComInterfaceType.InterfaceIsIUnknown)]
 # 指定 COM 接口类型为 IUnknown 接口
internal interface IExplorerBrowserEvents
{
    [PreserveSig]
    # 保留返回值签名
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    # 指定方法实现由运行时提供
    HResult OnNavigationPending(nint pidlFolder);
    # 处理导航操作即将开始的事件

    [PreserveSig]
    # 保留返回值签名
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    # 定义一个方法，当视图被创建时调用，参数是一个 COM 接口对象
    HResult OnViewCreated([MarshalAs(UnmanagedType.IUnknown)] object psv);

    # 定义一个方法，当导航完成时调用，参数是导航的文件夹 ID
    [PreserveSig]
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    HResult OnNavigationComplete(nint pidlFolder);

    # 定义一个方法，当导航失败时调用，参数是导航的文件夹 ID
    [PreserveSig]
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    HResult OnNavigationFailed(nint pidlFolder);
# 标记为 COM 导入的接口，使用 IUnknown 接口
[ComImport,
 Guid(ExplorerBrowserIIDGuid.IExplorerPaneVisibility),
 InterfaceType(ComInterfaceType.InterfaceIsIUnknown)]
internal interface IExplorerPaneVisibility
{
    # 保持方法签名
    [PreserveSig]
    # 方法实现调用
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    # 获取面板状态
    HResult GetPaneState(ref Guid explorerPane, out ExplorerPaneState peps);
};

# 标记为 COM 导入的接口，使用 IUnknown 接口
[ComImport,
 Guid(ExplorerBrowserIIDGuid.IFolderView),
 InterfaceType(ComInterfaceType.InterfaceIsIUnknown)]
internal interface IFolderView
{
    # 方法实现调用，获取当前视图模式
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    void GetCurrentViewMode([Out] out uint pViewMode);

    # 方法实现调用，设置当前视图模式
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    void SetCurrentViewMode(uint ViewMode);

    # 方法实现调用，获取文件夹对象
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    void GetFolder(ref Guid riid, [MarshalAs(UnmanagedType.IUnknown)] out object ppv);

    # 方法实现调用，获取指定索引项
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    void Item(int iItemIndex, out nint ppidl);

    # 方法实现调用，获取项数
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    void ItemCount(uint uFlags, out int pcItems);

    # 方法实现调用，获取多个项
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    void Items(uint uFlags, ref Guid riid, [Out, MarshalAs(UnmanagedType.IUnknown)] out object ppv);

    # 方法实现调用，获取标记项
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    void GetSelectionMarkedItem(out int piItem);

    # 方法实现调用，获取焦点项
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    void GetFocusedItem(out int piItem);

    # 方法实现调用，获取项位置
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    void GetItemPosition(nint pidl, out POINT ppt);

    # 方法实现调用，获取间距
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    void GetSpacing([Out] out POINT ppt);

    # 方法实现调用，获取默认间距
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    void GetDefaultSpacing(out POINT ppt);

    # 方法实现调用，获取自动排列设置
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    void GetAutoArrange();

    # 方法实现调用，选择项
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    void SelectItem(int iItem, uint dwFlags);

    # 方法实现调用，选择并定位项
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    void SelectAndPositionItems(uint cidl, nint apidl, ref POINT apt, uint dwFlags);
}

# 继承自 IFolderView 接口的 COM 导入接口
[ComImport,
 Guid(ExplorerBrowserIIDGuid.IFolderView2),
 InterfaceType(ComInterfaceType.InterfaceIsIUnknown)]
internal interface IFolderView2 : IFolderView
{
    # 保持方法签名
    [PreserveSig]
    # 方法实现调用，获取当前视图模式
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    HResult GetCurrentViewMode(out uint pViewMode);

    # 方法实现调用，设置当前视图模式
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    void SetCurrentViewMode(uint ViewMode);
    # 定义一个内部调用的方法，用于获取文件夹
        [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
        void GetFolder(ref Guid riid, [MarshalAs(UnmanagedType.IUnknown)] out object ppv);
    
    # 定义一个内部调用的方法，用于获取指定索引的项目
        [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
        void Item(int iItemIndex, out nint ppidl);
    
    # 定义一个带有保留标志的方法，用于获取项目数
        [PreserveSig]
        [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
        HResult ItemCount(uint uFlags, out int pcItems);
    
    # 定义一个带有保留标志的方法，用于获取项目集合
        [PreserveSig]
        [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
        HResult Items(uint uFlags, ref Guid riid, [Out, MarshalAs(UnmanagedType.IUnknown)] out object ppv);
    
    # 定义一个内部调用的方法，用于获取选中的项目
        [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
        void GetSelectionMarkedItem(out int piItem);
    
    # 定义一个内部调用的方法，用于获取焦点项目
        [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
        void GetFocusedItem(out int piItem);
    
    # 定义一个内部调用的方法，用于获取指定项目的位置
        [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
        void GetItemPosition(nint pidl, out POINT ppt);
    
    # 定义一个内部调用的方法，用于获取间距
        [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
        void GetSpacing([Out] out POINT ppt);
    
    # 定义一个内部调用的方法，用于获取默认间距
        [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
        void GetDefaultSpacing(out POINT ppt);
    
    # 定义一个内部调用的方法，用于获取自动排列的状态
        [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
        void GetAutoArrange();
    
    # 定义一个内部调用的方法，用于选择项目
        [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
        void SelectItem(int iItem, uint dwFlags);
    
    # 定义一个内部调用的方法，用于选择并定位多个项目
        [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
        void SelectAndPositionItems(uint cidl, nint apidl, ref POINT apt, uint dwFlags);
    
    # 定义一个内部调用的方法，用于设置分组方式
        [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
        void SetGroupBy(nint key, bool fAscending);
    
    # 定义一个内部调用的方法，用于获取分组方式
        [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
        void GetGroupBy(ref nint pkey, ref bool pfAscending);
    
    # 定义一个内部调用的方法，用于设置视图属性
        [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
        void SetViewProperty(nint pidl, nint propkey, object propvar);
    
    # 定义一个内部调用的方法，用于获取视图属性
        [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
        void GetViewProperty(nint pidl, nint propkey, out object ppropvar);
    
    # 定义一个内部调用的方法，用于设置瓷砖视图属性
        [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
        void SetTileViewProperties(nint pidl, [MarshalAs(UnmanagedType.LPWStr)] string pszPropList);
    
    # 定义一个内部调用的方法，用于设置扩展瓷砖视图属性
        [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
        void SetExtendedTileViewProperties(nint pidl, [MarshalAs(UnmanagedType.LPWStr)] string pszPropList);
    
    # 定义一个内部调用的方法，用于设置文本
        [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
        void SetText(int iType, [MarshalAs(UnmanagedType.LPWStr)] string pwszText);
    # 通过内部调用设置当前文件夹的标志
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    void SetCurrentFolderFlags(uint dwMask, uint dwFlags);

    # 通过内部调用获取当前文件夹的标志
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    void GetCurrentFolderFlags(out uint pdwFlags);

    # 通过内部调用获取当前排序列的数量
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    void GetSortColumnCount(out int pcColumns);

    # 通过内部调用设置排序列
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    void SetSortColumns(nint rgSortColumns, int cColumns);

    # 通过内部调用获取排序列
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    void GetSortColumns(out nint rgSortColumns, int cColumns);

    # 通过内部调用获取指定项的信息
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    void GetItem(int iItem, ref Guid riid, [MarshalAs(UnmanagedType.IUnknown)] out object ppv);

    # 通过内部调用获取当前可见的项
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    void GetVisibleItem(int iStart, bool fPrevious, out int piItem);

    # 通过内部调用获取当前选中的项
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    void GetSelectedItem(int iStart, out int piItem);

    # 通过内部调用获取当前选择的项目集合
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    void GetSelection(bool fNoneImpliesFolder, out IShellItemArray ppsia);

    # 通过内部调用获取指定项目的选择状态
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    void GetSelectionState(nint pidl, out uint pdwFlags);

    # 通过内部调用对选中的项目执行指定的操作
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    void InvokeVerbOnSelection([In, MarshalAs(UnmanagedType.LPWStr)] string pszVerb);

    # 保留签名，设置视图模式和图标大小，返回操作结果
    [PreserveSig]
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    HResult SetViewModeAndIconSize(int uViewMode, int iImageSize);

    # 保留签名，获取视图模式和图标大小，返回操作结果
    [PreserveSig]
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    HResult GetViewModeAndIconSize(out int puViewMode, out int piImageSize);

    # 通过内部调用设置分组子集的数量
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    void SetGroupSubsetCount(uint cVisibleRows);

    # 通过内部调用获取分组子集的数量
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    void GetGroupSubsetCount(out uint pcVisibleRows);

    # 通过内部调用设置是否重绘
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    void SetRedraw(bool fRedrawOn);

    # 通过内部调用检查是否在同一文件夹中移动
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    void IsMoveInSameFolder();

    # 通过内部调用执行重命名操作
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    void DoRename();
# 定义一个 COM 接口 IInputObject，提供用于界面激活、焦点检查和消息翻译的方法
[ComImport,
 Guid(ExplorerBrowserIIDGuid.IInputObject),
 InterfaceType(ComInterfaceType.InterfaceIsIUnknown)]
internal interface IInputObject
{
    # 保留方法签名特性，指定方法实现调用的方式为 Runtime
    [PreserveSig]
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    HResult UIActivateIO(bool fActivate, ref System.Windows.Forms.Message pMsg);

    # 保留方法签名特性，指定方法实现调用的方式为 Runtime
    [PreserveSig]
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    HResult HasFocusIO();

    # 保留方法签名特性，指定方法实现调用的方式为 Runtime
    [PreserveSig]
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    HResult TranslateAcceleratorIO(ref System.Windows.Forms.Message pMsg);
};

# 定义一个 COM 接口 IServiceProvider，提供服务查询方法
[ComImport,
 Guid(ExplorerBrowserIIDGuid.IServiceProvider),
 InterfaceType(ComInterfaceType.InterfaceIsIUnknown)]
internal interface IServiceProvider
{
    # 保留方法签名特性，指定方法实现调用的方式为 InternalCall
    [PreserveSig]
    [MethodImpl(MethodImplOptions.InternalCall)]
    HResult QueryService(ref Guid guidService, ref Guid riid, out nint ppvObject);
};

# 定义一个 COM 接口 IShellView，提供对 Shell 视图的各种操作方法
[ComImport,
 Guid(ExplorerBrowserIIDGuid.IShellView),
 InterfaceType(ComInterfaceType.InterfaceIsIUnknown)]
internal interface IShellView
{
    # 保留方法签名特性，指定方法实现调用的方式为 Runtime
    [PreserveSig]
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    HResult GetWindow(
        out nint phwnd);

    # 保留方法签名特性，指定方法实现调用的方式为 Runtime
    [PreserveSig]
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    HResult ContextSensitiveHelp(
        bool fEnterMode);

    # 保留方法签名特性，指定方法实现调用的方式为 Runtime
    [PreserveSig]
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    HResult TranslateAccelerator(
        nint pmsg);

    # 保留方法签名特性，指定方法实现调用的方式为 Runtime
    [PreserveSig]
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    HResult EnableModeless(
        bool fEnable);

    # 保留方法签名特性，指定方法实现调用的方式为 Runtime
    [PreserveSig]
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    HResult UIActivate(
        uint uState);

    # 保留方法签名特性，指定方法实现调用的方式为 Runtime
    [PreserveSig]
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    HResult Refresh();

    # 保留方法签名特性，指定方法实现调用的方式为 Runtime
    [PreserveSig]
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    HResult CreateViewWindow(
        [MarshalAs(UnmanagedType.IUnknown)] object psvPrevious,
        nint pfs,
        [MarshalAs(UnmanagedType.IUnknown)] object psb,
        nint prcView,
        out nint phWnd);

    # 保留方法签名特性，指定方法实现调用的方式为 Runtime
    [PreserveSig]
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    HResult DestroyViewWindow();

    # 保留方法签名特性，指定方法实现调用的方式为 Runtime
    [PreserveSig]
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    HResult GetCurrentInfo(
        out nint pfs);

    # 保留方法签名特性，指定方法实现调用的方式为 Runtime
    [PreserveSig]
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    HResult AddPropertySheetPages(
        uint dwReserved,
        nint pfn,
        uint lparam);

    # 保留方法签名特性，指定方法实现调用的方式为 Runtime
    [PreserveSig]
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    # 保存视图状态，返回结果指示操作成功与否
    HResult SaveViewState();

    # 保留签名，定义一个选择项目的方法，使用运行时内部调用
    [PreserveSig]
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    HResult SelectItem(
        # 项目的标识符
        nint pidlItem,
        # 选择操作的标志
        uint uFlags);

    # 保留签名，定义一个获取项目对象的方法，使用运行时内部调用
    [PreserveSig]
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    HResult GetItemObject(
        # 要获取的对象类型
        ShellViewGetItemObject uItem,
        # 接收 GUID 的引用
        ref Guid riid,
        # 返回的对象接口
        [MarshalAs(UnmanagedType.IUnknown)] out object ppv);
}
# 以上代码结束了类或接口定义的主体部分

[ComImport,
 # 指示该类是由 COM（组件对象模型）创建的
 TypeLibType(TypeLibTypeFlags.FCanCreate),
 # 指示该类可以被创建
 ClassInterface(ClassInterfaceType.None),
 # 指示不自动生成类接口
 Guid(ExplorerBrowserCLSIDGuid.ExplorerBrowser)]
 # 指定该类的 GUID
internal class ExplorerBrowserClass : IExplorerBrowser
{
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    # 指定该方法在运行时由内部调用
    public extern virtual void Initialize(nint hwndParent, [In] ref RECT prc, [In] FolderSettings pfs);
    # 初始化浏览器对象，接受父窗口句柄、区域和文件夹设置

    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    # 指定该方法在运行时由内部调用
    public extern virtual void Destroy();
    # 销毁浏览器对象

    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    # 指定该方法在运行时由内部调用
    public extern virtual void SetRect([In, Out] ref nint phdwp, RECT rcBrowser);
    # 设置浏览器对象的矩形区域

    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    # 指定该方法在运行时由内部调用
    public extern virtual void SetPropertyBag([MarshalAs(UnmanagedType.LPWStr)] string pszPropertyBag);
    # 设置属性袋，属性袋是一个字符串，表示浏览器对象的属性集合

    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    # 指定该方法在运行时由内部调用
    public extern virtual void SetEmptyText([MarshalAs(UnmanagedType.LPWStr)] string pszEmptyText);
    # 设置当浏览器为空时显示的文本

    [PreserveSig]
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    # 保持方法签名不变，指定该方法在运行时由内部调用
    public extern virtual HResult SetFolderSettings(FolderSettings pfs);
    # 设置文件夹的显示设置，返回操作结果

    [PreserveSig]
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    # 保持方法签名不变，指定该方法在运行时由内部调用
    public extern virtual HResult Advise(nint psbe, out uint pdwCookie);
    # 注册事件通知，返回操作结果和通知的唯一标识符

    [PreserveSig]
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    # 保持方法签名不变，指定该方法在运行时由内部调用
    public extern virtual HResult Unadvise(uint dwCookie);
    # 注销事件通知，使用之前获得的唯一标识符，返回操作结果

    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    # 指定该方法在运行时由内部调用
    public extern virtual void SetOptions([In] ExplorerBrowserOptions dwFlag);
    # 设置浏览器的选项

    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    # 指定该方法在运行时由内部调用
    public extern virtual void GetOptions(out ExplorerBrowserOptions pdwFlag);
    # 获取当前浏览器的选项

    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    # 指定该方法在运行时由内部调用
    public extern virtual void BrowseToIDList(nint pidl, uint uFlags);
    # 根据标识符列表（IDList）浏览到指定位置

    [PreserveSig]
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    # 保持方法签名不变，指定该方法在运行时由内部调用
    public extern virtual HResult BrowseToObject([MarshalAs(UnmanagedType.IUnknown)] object punk, uint uFlags);
    # 根据对象浏览到指定位置，返回操作结果

    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    # 指定该方法在运行时由内部调用
    public extern virtual void FillFromObject([MarshalAs(UnmanagedType.IUnknown)] object punk, int dwFlags);
    # 从对象填充浏览器内容

    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    # 指定该方法在运行时由内部调用
    public extern virtual void RemoveAll();
    # 移除浏览器中的所有内容

    [PreserveSig]
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    # 保持方法签名不变，指定该方法在运行时由内部调用
    public extern virtual HResult GetCurrentView(ref Guid riid, out nint ppv);
    # 获取当前视图，返回操作结果和视图的指针
}

[StructLayout(LayoutKind.Sequential, Pack = 4)]
# 指定结构体的布局方式，按顺序排列，数据对齐到4字节边界
internal class FolderSettings
{
    public FolderViewMode ViewMode;
    # 文件夹视图模式设置
    // 声明一个公有的 FolderOptions 类型的字段
    public FolderOptions Options;
# 代码块的结束标记
}
```