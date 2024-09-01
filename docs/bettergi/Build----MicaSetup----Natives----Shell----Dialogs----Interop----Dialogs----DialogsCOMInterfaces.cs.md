# `.\better-genshin-impact\Build\MicaSetup\Natives\Shell\Dialogs\Interop\Dialogs\DialogsCOMInterfaces.cs`

```cs
using System; // 引入 System 命名空间，提供基本的系统功能
using System.Runtime.CompilerServices; // 引入 Runtime.CompilerServices 命名空间，提供编译器服务
using System.Runtime.InteropServices; // 引入 Runtime.InteropServices 命名空间，提供与 COM 交互的功能

namespace MicaSetup.Shell.Dialogs; // 定义一个名为 MicaSetup.Shell.Dialogs 的命名空间

#pragma warning disable CS0108 // 禁用编译器警告 CS0108，避免名称隐藏警告

[ComImport(), // 标记该接口为 COM 导入类型
Guid(ShellIIDGuid.IFileDialog), // 指定该接口的 COM GUID
InterfaceType(ComInterfaceType.InterfaceIsIUnknown)] // 指定接口类型为 IUnknown
internal interface IFileDialog : IModalWindow // 定义 IFileDialog 接口，继承 IModalWindow
{
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime), // 标记方法实现为内部调用
    PreserveSig] // 保留方法签名，避免 COM 的 HRESULT 错误处理
    int Show([In] nint parent); // 显示对话框，接受父窗口句柄

    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)] // 标记方法实现为内部调用
    void SetFileTypes(
        [In] uint cFileTypes, // 文件类型的数量
        [In, MarshalAs(UnmanagedType.LPArray)] FilterSpec[] rgFilterSpec); // 文件类型过滤器数组

    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)] // 标记方法实现为内部调用
    void SetFileTypeIndex([In] uint iFileType); // 设置当前文件类型索引

    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)] // 标记方法实现为内部调用
    void GetFileTypeIndex(out uint piFileType); // 获取当前文件类型索引

    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)] // 标记方法实现为内部调用
    void Advise(
        [In, MarshalAs(UnmanagedType.Interface)] IFileDialogEvents pfde, // 指定事件接口
        out uint pdwCookie); // 接收事件订阅的 cookie

    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)] // 标记方法实现为内部调用
    void Unadvise([In] uint dwCookie); // 取消事件订阅

    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)] // 标记方法实现为内部调用
    void SetOptions([In] FileOpenOptions fos); // 设置对话框选项

    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)] // 标记方法实现为内部调用
    void GetOptions(out FileOpenOptions pfos); // 获取对话框选项

    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)] // 标记方法实现为内部调用
    void SetDefaultFolder([In, MarshalAs(UnmanagedType.Interface)] IShellItem psi); // 设置默认文件夹

    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)] // 标记方法实现为内部调用
    void SetFolder([In, MarshalAs(UnmanagedType.Interface)] IShellItem psi); // 设置当前文件夹

    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)] // 标记方法实现为内部调用
    void GetFolder([MarshalAs(UnmanagedType.Interface)] out IShellItem ppsi); // 获取当前文件夹

    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)] // 标记方法实现为内部调用
    void GetCurrentSelection([MarshalAs(UnmanagedType.Interface)] out IShellItem ppsi); // 获取当前选择的项

    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)] // 标记方法实现为内部调用
    void SetFileName([In, MarshalAs(UnmanagedType.LPWStr)] string pszName); // 设置文件名

    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)] // 标记方法实现为内部调用
    void GetFileName([MarshalAs(UnmanagedType.LPWStr)] out string pszName); // 获取文件名

    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)] // 标记方法实现为内部调用
    void SetTitle([In, MarshalAs(UnmanagedType.LPWStr)] string pszTitle); // 设置对话框标题

    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)] // 标记方法实现为内部调用
    void SetOkButtonLabel([In, MarshalAs(UnmanagedType.LPWStr)] string pszText); // 设置“确定”按钮的标签
    # 定义一个方法，声明为内部调用，运行时代码类型
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    # 设置文件名标签，pszLabel 是要设置的标签文本
    void SetFileNameLabel([In, MarshalAs(UnmanagedType.LPWStr)] string pszLabel);
    
    # 定义一个方法，声明为内部调用，运行时代码类型
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    # 获取结果，返回一个 IShellItem 接口对象，通过输出参数 ppsi
    void GetResult([MarshalAs(UnmanagedType.Interface)] out IShellItem ppsi);
    
    # 定义一个方法，声明为内部调用，运行时代码类型
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    # 添加位置，psi 是 IShellItem 接口对象，fdap 是 FileDialogAddPlacement 枚举值
    void AddPlace([In, MarshalAs(UnmanagedType.Interface)] IShellItem psi, FileDialogAddPlacement fdap);
    
    # 定义一个方法，声明为内部调用，运行时代码类型
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    # 设置默认扩展名，pszDefaultExtension 是要设置的扩展名
    void SetDefaultExtension([In, MarshalAs(UnmanagedType.LPWStr)] string pszDefaultExtension);
    
    # 定义一个方法，声明为内部调用，运行时代码类型
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    # 关闭，hr 是一个错误代码，指示操作的结果
    void Close([MarshalAs(UnmanagedType.Error)] int hr);
    
    # 定义一个方法，声明为内部调用，运行时代码类型
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    # 设置客户端 GUID，guid 是用来标识客户端的 GUID
    void SetClientGuid([In] ref Guid guid);
    
    # 定义一个方法，声明为内部调用，运行时代码类型
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    # 清除客户端数据，不需要参数
    void ClearClientData();
    
    # 定义一个方法，声明为内部调用，运行时代码类型
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    # 设置过滤器，pFilter 是过滤器接口的指针
    void SetFilter([MarshalAs(UnmanagedType.Interface)] nint pFilter);
}
# 结束大括号，标志接口定义的结束

[ComImport,
# 指示该接口需要通过 COM 进行导入
Guid(ShellIIDGuid.IFileDialogControlEvents),
# 定义接口的 GUID
InterfaceType(ComInterfaceType.InterfaceIsIUnknown)]
# 指定接口类型为 IUnknown 类型
internal interface IFileDialogControlEvents
{
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    # 指示方法通过 COM 调用
    void OnItemSelected(
        [In, MarshalAs(UnmanagedType.Interface)] IFileDialogCustomize pfdc,
        [In] int dwIDCtl,
        [In] int dwIDItem);
    # 处理项目选择事件

    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    void OnButtonClicked(
        [In, MarshalAs(UnmanagedType.Interface)] IFileDialogCustomize pfdc,
        [In] int dwIDCtl);
    # 处理按钮点击事件

    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    void OnCheckButtonToggled(
        [In, MarshalAs(UnmanagedType.Interface)] IFileDialogCustomize pfdc,
        [In] int dwIDCtl,
        [In] bool bChecked);
    # 处理复选框切换事件

    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    void OnControlActivating(
        [In, MarshalAs(UnmanagedType.Interface)] IFileDialogCustomize pfdc,
        [In] int dwIDCtl);
    # 处理控件激活事件
}

[ComImport,
# 指示该接口需要通过 COM 进行导入
Guid(ShellIIDGuid.IFileDialogCustomize),
# 定义接口的 GUID
InterfaceType(ComInterfaceType.InterfaceIsIUnknown)]
# 指定接口类型为 IUnknown 类型
internal interface IFileDialogCustomize
{
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    void EnableOpenDropDown([In] int dwIDCtl);
    # 启用下拉菜单

    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    void AddMenu(
        [In] int dwIDCtl,
        [In, MarshalAs(UnmanagedType.LPWStr)] string pszLabel);
    # 添加菜单项

    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    void AddPushButton(
        [In] int dwIDCtl,
        [In, MarshalAs(UnmanagedType.LPWStr)] string pszLabel);
    # 添加按钮

    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    void AddComboBox([In] int dwIDCtl);
    # 添加组合框

    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    void AddRadioButtonList([In] int dwIDCtl);
    # 添加单选按钮列表

    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    void AddCheckButton(
        [In] int dwIDCtl,
        [In, MarshalAs(UnmanagedType.LPWStr)] string pszLabel,
        [In] bool bChecked);
    # 添加复选框

    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    void AddEditBox(
        [In] int dwIDCtl,
        [In, MarshalAs(UnmanagedType.LPWStr)] string pszText);
    # 添加文本框

    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    void AddSeparator([In] int dwIDCtl);
    # 添加分隔符

    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    void AddText(
        [In] int dwIDCtl,
        [In, MarshalAs(UnmanagedType.LPWStr)] string pszText);
    # 添加文本标签

    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    # 设置控件的标签文本
    void SetControlLabel(
        [In] int dwIDCtl,  # 控件 ID
        [In, MarshalAs(UnmanagedType.LPWStr)] string pszLabel);  # 标签文本

    # 获取控件的状态
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    void GetControlState(
        [In] int dwIDCtl,  # 控件 ID
        [Out] out ControlState pdwState);  # 控件状态

    # 设置控件的状态
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    void SetControlState(
        [In] int dwIDCtl,  # 控件 ID
        [In] ControlState dwState);  # 控件状态

    # 获取编辑框的文本
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    void GetEditBoxText(
        [In] int dwIDCtl,  # 控件 ID
        [MarshalAs(UnmanagedType.LPWStr)] out string ppszText);  # 编辑框文本

    # 设置编辑框的文本
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    void SetEditBoxText(
        [In] int dwIDCtl,  # 控件 ID
        [In, MarshalAs(UnmanagedType.LPWStr)] string pszText);  # 编辑框文本

    # 获取复选按钮的状态
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    void GetCheckButtonState(
        [In] int dwIDCtl,  # 控件 ID
        [Out] out bool pbChecked);  # 复选按钮状态

    # 设置复选按钮的状态
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    void SetCheckButtonState(
        [In] int dwIDCtl,  # 控件 ID
        [In] bool bChecked);  # 复选按钮状态

    # 向控件添加一个项
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    void AddControlItem(
        [In] int dwIDCtl,  # 控件 ID
        [In] int dwIDItem,  # 项 ID
        [In, MarshalAs(UnmanagedType.LPWStr)] string pszLabel);  # 项的标签文本

    # 从控件中移除一个项
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    void RemoveControlItem(
        [In] int dwIDCtl,  # 控件 ID
        [In] int dwIDItem);  # 项 ID

    # 从控件中移除所有项
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    void RemoveAllControlItems([In] int dwIDCtl);  # 控件 ID

    # 获取控件中项的状态
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    void GetControlItemState(
        [In] int dwIDCtl,  # 控件 ID
        [In] int dwIDItem,  # 项 ID
        [Out] out ControlState pdwState);  # 项的状态

    # 设置控件中项的状态
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    void SetControlItemState(
        [In] int dwIDCtl,  # 控件 ID
        [In] int dwIDItem,  # 项 ID
        [In] ControlState dwState);  # 项的状态

    # 获取选定的控件项
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    void GetSelectedControlItem(
        [In] int dwIDCtl,  # 控件 ID
        [Out] out int pdwIDItem);  # 选定的项 ID

    # 设置选定的控件项
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    void SetSelectedControlItem(
        [In] int dwIDCtl,  # 控件 ID
        [In] int dwIDItem);  # 项 ID

    # 开始一个可视组
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    void StartVisualGroup(
        [In] int dwIDCtl,  # 控件 ID
        [In, MarshalAs(UnmanagedType.LPWStr)] string pszLabel);  # 组的标签文本

    # 结束一个可视组
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    void EndVisualGroup();  # 结束可视组
    # 指定方法实现方式为内部调用，这意味着该方法由运行时提供实现
        [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
        # 定义一个无返回值的方法，方法参数为一个整型变量，标记为输入参数
        void MakeProminent([In] int dwIDCtl);
# 这是一个 COM 接口定义，描述了文件对话框事件的处理方法
[ComImport,
Guid(ShellIIDGuid.IFileDialogEvents),
InterfaceType(ComInterfaceType.InterfaceIsIUnknown)]
internal interface IFileDialogEvents
{
    # 文件对话框 OK 按钮点击事件
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime),
    PreserveSig]
    HResult OnFileOk([In, MarshalAs(UnmanagedType.Interface)] IFileDialog pfd);

    # 文件夹更改前的事件
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime),
    PreserveSig]
    HResult OnFolderChanging(
        [In, MarshalAs(UnmanagedType.Interface)] IFileDialog pfd,
        [In, MarshalAs(UnmanagedType.Interface)] IShellItem psiFolder);

    # 文件夹更改事件
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    void OnFolderChange([In, MarshalAs(UnmanagedType.Interface)] IFileDialog pfd);

    # 选择更改事件
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    void OnSelectionChange([In, MarshalAs(UnmanagedType.Interface)] IFileDialog pfd);

    # 文件共享冲突事件
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    void OnShareViolation(
        [In, MarshalAs(UnmanagedType.Interface)] IFileDialog pfd,
        [In, MarshalAs(UnmanagedType.Interface)] IShellItem psi,
        out FileDialogEventShareViolationResponse pResponse);

    # 文件类型更改事件
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    void OnTypeChange([In, MarshalAs(UnmanagedType.Interface)] IFileDialog pfd);

    # 文件覆盖事件
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    void OnOverwrite([In, MarshalAs(UnmanagedType.Interface)] IFileDialog pfd,
        [In, MarshalAs(UnmanagedType.Interface)] IShellItem psi,
        out FileDialogEventOverwriteResponse pResponse);
}

# 这是一个 COM 接口定义，描述了文件打开对话框的功能
[ComImport(),
            Guid(ShellIIDGuid.IFileOpenDialog),
InterfaceType(ComInterfaceType.InterfaceIsIUnknown)]
internal interface IFileOpenDialog : IFileDialog
{
    # 显示文件打开对话框
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime),
    PreserveSig]
    int Show([In] nint parent);

    # 设置文件类型过滤器
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    void SetFileTypes([In] uint cFileTypes, [In] ref FilterSpec rgFilterSpec);

    # 设置当前文件类型索引
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    void SetFileTypeIndex([In] uint iFileType);

    # 获取当前文件类型索引
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    void GetFileTypeIndex(out uint piFileType);

    # 注册事件监听器
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    void Advise(
        [In, MarshalAs(UnmanagedType.Interface)] IFileDialogEvents pfde,
        out uint pdwCookie);

    # 注销事件监听器
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    void Unadvise([In] uint dwCookie);

    # 设置对话框选项
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    void SetOptions([In] FileOpenOptions fos);
    # 指示方法为内部调用，且代码类型为运行时
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    # 声明一个方法，用于获取选项，输出参数为 FileOpenOptions 类型
    void GetOptions(out FileOpenOptions pfos);
    
    # 指示方法为内部调用，且代码类型为运行时
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    # 声明一个方法，用于设置默认文件夹，输入参数为 IShellItem 类型
    void SetDefaultFolder([In, MarshalAs(UnmanagedType.Interface)] IShellItem psi);
    
    # 指示方法为内部调用，且代码类型为运行时
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    # 声明一个方法，用于设置文件夹，输入参数为 IShellItem 类型
    void SetFolder([In, MarshalAs(UnmanagedType.Interface)] IShellItem psi);
    
    # 指示方法为内部调用，且代码类型为运行时
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    # 声明一个方法，用于获取文件夹，输出参数为 IShellItem 类型
    void GetFolder([MarshalAs(UnmanagedType.Interface)] out IShellItem ppsi);
    
    # 指示方法为内部调用，且代码类型为运行时
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    # 声明一个方法，用于获取当前选择项，输出参数为 IShellItem 类型
    void GetCurrentSelection([MarshalAs(UnmanagedType.Interface)] out IShellItem ppsi);
    
    # 指示方法为内部调用，且代码类型为运行时
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    # 声明一个方法，用于设置文件名，输入参数为字符串类型
    void SetFileName([In, MarshalAs(UnmanagedType.LPWStr)] string pszName);
    
    # 指示方法为内部调用，且代码类型为运行时
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    # 声明一个方法，用于获取文件名，输出参数为字符串类型
    void GetFileName([MarshalAs(UnmanagedType.LPWStr)] out string pszName);
    
    # 指示方法为内部调用，且代码类型为运行时
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    # 声明一个方法，用于设置标题，输入参数为字符串类型
    void SetTitle([In, MarshalAs(UnmanagedType.LPWStr)] string pszTitle);
    
    # 指示方法为内部调用，且代码类型为运行时
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    # 声明一个方法，用于设置确认按钮的标签，输入参数为字符串类型
    void SetOkButtonLabel([In, MarshalAs(UnmanagedType.LPWStr)] string pszText);
    
    # 指示方法为内部调用，且代码类型为运行时
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    # 声明一个方法，用于设置文件名标签，输入参数为字符串类型
    void SetFileNameLabel([In, MarshalAs(UnmanagedType.LPWStr)] string pszLabel);
    
    # 指示方法为内部调用，且代码类型为运行时
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    # 声明一个方法，用于获取结果，输出参数为 IShellItem 类型
    void GetResult([MarshalAs(UnmanagedType.Interface)] out IShellItem ppsi);
    
    # 指示方法为内部调用，且代码类型为运行时
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    # 声明一个方法，用于添加位置，输入参数为 IShellItem 和 FileDialogAddPlacement 类型
    void AddPlace([In, MarshalAs(UnmanagedType.Interface)] IShellItem psi, FileDialogAddPlacement fdap);
    
    # 指示方法为内部调用，且代码类型为运行时
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    # 声明一个方法，用于设置默认扩展名，输入参数为字符串类型
    void SetDefaultExtension([In, MarshalAs(UnmanagedType.LPWStr)] string pszDefaultExtension);
    
    # 指示方法为内部调用，且代码类型为运行时
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    # 声明一个方法，用于关闭对话框，输入参数为错误码
    void Close([MarshalAs(UnmanagedType.Error)] int hr);
    
    # 指示方法为内部调用，且代码类型为运行时
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    # 声明一个方法，用于设置客户端 GUID，输入参数为 Guid 类型的引用
    void SetClientGuid([In] ref Guid guid);
    
    # 指示方法为内部调用，且代码类型为运行时
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    # 声明一个方法，用于清除客户端数据
    void ClearClientData();
    
    # 指示方法为内部调用，且代码类型为运行时
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    # 声明一个方法，用于设置过滤器，输入参数为 nint 类型
    void SetFilter([MarshalAs(UnmanagedType.Interface)] nint pFilter);
    
    # 指示方法为内部调用，且代码类型为运行时
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    # 声明一个方法，用于获取结果数组，输出参数为 IShellItemArray 类型
    void GetResults([MarshalAs(UnmanagedType.Interface)] out IShellItemArray ppenum);
    # 指定方法的实现细节为内部调用，并定义代码类型为运行时
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    # 声明一个方法，用于获取选定的项目
    void GetSelectedItems([MarshalAs(UnmanagedType.Interface)] out IShellItemArray ppsai);
    # 方法参数为一个未初始化的 IShellItemArray 接口，方法将其作为输出参数
}
# 结束前一个接口或类的定义

[ComImport(),
# 标记该接口为 COM 组件，允许通过 COM 进行互操作
Guid(ShellIIDGuid.IFileSaveDialog),
# 定义该接口的唯一标识符
InterfaceType(ComInterfaceType.InterfaceIsIUnknown)]
# 指定该接口使用 IUnknown 接口类型
internal interface IFileSaveDialog : IFileDialog
# 声明一个内部接口 IFileSaveDialog，继承自 IFileDialog
{
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime),
    PreserveSig]
    # 指定该方法为内部调用，并保持签名
    int Show([In] nint parent);
    # 显示文件保存对话框，parent 参数为对话框的父窗口

    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    # 指定该方法为内部调用
    void SetFileTypes(
        [In] uint cFileTypes,
        [In] ref FilterSpec rgFilterSpec);
    # 设置文件对话框支持的文件类型

    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    # 指定该方法为内部调用
    void SetFileTypeIndex([In] uint iFileType);
    # 设置当前选择的文件类型索引

    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    # 指定该方法为内部调用
    void GetFileTypeIndex(out uint piFileType);
    # 获取当前选择的文件类型索引

    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    # 指定该方法为内部调用
    void Advise(
        [In, MarshalAs(UnmanagedType.Interface)] IFileDialogEvents pfde,
        out uint pdwCookie);
    # 注册对话框事件的回调接口，并返回一个 cookie 以用于取消注册

    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    # 指定该方法为内部调用
    void Unadvise([In] uint dwCookie);
    # 取消对话框事件的注册

    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    # 指定该方法为内部调用
    void SetOptions([In] FileOpenOptions fos);
    # 设置对话框的选项

    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    # 指定该方法为内部调用
    void GetOptions(out FileOpenOptions pfos);
    # 获取当前对话框的选项

    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    # 指定该方法为内部调用
    void SetDefaultFolder([In, MarshalAs(UnmanagedType.Interface)] IShellItem psi);
    # 设置对话框的默认文件夹

    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    # 指定该方法为内部调用
    void SetFolder([In, MarshalAs(UnmanagedType.Interface)] IShellItem psi);
    # 设置对话框当前的文件夹

    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    # 指定该方法为内部调用
    void GetFolder([MarshalAs(UnmanagedType.Interface)] out IShellItem ppsi);
    # 获取对话框当前的文件夹

    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    # 指定该方法为内部调用
    void GetCurrentSelection([MarshalAs(UnmanagedType.Interface)] out IShellItem ppsi);
    # 获取对话框当前选择的项

    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    # 指定该方法为内部调用
    void SetFileName([In, MarshalAs(UnmanagedType.LPWStr)] string pszName);
    # 设置对话框中的文件名

    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    # 指定该方法为内部调用
    void GetFileName([MarshalAs(UnmanagedType.LPWStr)] out string pszName);
    # 获取对话框中的文件名

    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    # 指定该方法为内部调用
    void SetTitle([In, MarshalAs(UnmanagedType.LPWStr)] string pszTitle);
    # 设置对话框的标题

    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    # 指定该方法为内部调用
    void SetOkButtonLabel([In, MarshalAs(UnmanagedType.LPWStr)] string pszText);
    # 设置对话框 OK 按钮的标签

    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    # 指定该方法为内部调用
    void SetFileNameLabel([In, MarshalAs(UnmanagedType.LPWStr)] string pszLabel);
    # 设置对话框中文件名输入框的标签
    # 定义一个内部调用的方法，获取结果，结果通过 IShellItem 接口输出
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    void GetResult([MarshalAs(UnmanagedType.Interface)] out IShellItem ppsi);

    # 定义一个内部调用的方法，将 IShellItem 对象添加到对话框中，并指定文件对话框的放置选项
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    void AddPlace(
        [In, MarshalAs(UnmanagedType.Interface)] IShellItem psi,
        FileDialogAddPlacement fdap);

    # 定义一个内部调用的方法，设置默认的文件扩展名
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    void SetDefaultExtension([In, MarshalAs(UnmanagedType.LPWStr)] string pszDefaultExtension);

    # 定义一个内部调用的方法，关闭对话框并传递错误代码
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    void Close([MarshalAs(UnmanagedType.Error)] int hr);

    # 定义一个内部调用的方法，设置客户端的 GUID
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    void SetClientGuid([In] ref Guid guid);

    # 定义一个内部调用的方法，清除客户端数据
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    void ClearClientData();

    # 定义一个内部调用的方法，设置筛选器
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    void SetFilter([MarshalAs(UnmanagedType.Interface)] nint pFilter);

    # 定义一个内部调用的方法，设置“另存为”项
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    void SetSaveAsItem([In, MarshalAs(UnmanagedType.Interface)] IShellItem psi);

    # 定义一个内部调用的方法，设置属性存储
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    void SetProperties([In, MarshalAs(UnmanagedType.Interface)] nint pStore);

    # 定义一个内部调用的方法，设置已收集的属性，返回操作结果
    [PreserveSig]
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    int SetCollectedProperties(
        [In] IPropertyDescriptionList pList,
        [In] bool fAppendDefault);

    # 定义一个内部调用的方法，获取属性存储，并返回操作结果
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    [PreserveSig]
    HResult GetProperties(out IPropertyStore ppStore);

    # 定义一个内部调用的方法，应用属性到指定的 IShellItem 对象
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    void ApplyProperties(
        [In, MarshalAs(UnmanagedType.Interface)] IShellItem psi,
        [In, MarshalAs(UnmanagedType.Interface)] nint pStore,
        [In, ComAliasName("ShellObjects.wireHWND")] ref nint hwnd,
        [In, MarshalAs(UnmanagedType.Interface)] nint pSink);
# 结束上一个代码块的右花括号
}

# 恢复之前被禁用的警告 0108 的显示
#pragma warning restore 0108
```