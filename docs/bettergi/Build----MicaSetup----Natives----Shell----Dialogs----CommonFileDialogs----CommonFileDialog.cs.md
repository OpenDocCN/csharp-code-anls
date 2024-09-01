# `.\better-genshin-impact\Build\MicaSetup\Natives\Shell\Dialogs\CommonFileDialogs\CommonFileDialog.cs`

```cs
using MicaSetup.Helper; // 引入 MicaSetup.Helper 命名空间
using System; // 引入 System 命名空间
using System.Collections.Generic; // 引入 System.Collections.Generic 命名空间
using System.Collections.ObjectModel; // 引入 System.Collections.ObjectModel 命名空间
using System.ComponentModel; // 引入 System.ComponentModel 命名空间
using System.Diagnostics; // 引入 System.Diagnostics 命名空间
using System.Linq; // 引入 System.Linq 命名空间
using System.Runtime.InteropServices; // 引入 System.Runtime.InteropServices 命名空间
using System.Windows; // 引入 System.Windows 命名空间
using System.Windows.Interop; // 引入 System.Windows.Interop 命名空间
using System.Windows.Markup; // 引入 System.Windows.Markup 命名空间

namespace MicaSetup.Shell.Dialogs; // 定义 MicaSetup.Shell.Dialogs 命名空间

#pragma warning disable CS8618 // 禁用 CS8618 警告（未处理的非空引用警告）

[ContentProperty("Controls")] // 指定 ContentProperty 特性，标记 "Controls" 属性为 XAML 内容属性
public abstract class CommonFileDialog : IDialogControlHost, IDisposable // 定义 CommonFileDialog 抽象类，实现 IDialogControlHost 和 IDisposable 接口
{
    internal readonly Collection<IShellItem> items; // 定义只读的 internal Collection<IShellItem> 字段，用于存储 Shell 项

    internal DialogShowState showState = DialogShowState.PreShow; // 定义 internal 字段，用于存储对话框的显示状态，初始化为 PreShow

    private readonly CommonFileDialogControlCollection<CommonFileDialogControl> controls; // 定义只读字段，用于存储对话框控件的集合

    private readonly Collection<string> filenames; // 定义只读字段，用于存储文件名的集合

    private readonly CommonFileDialogFilterCollection filters; // 定义只读字段，用于存储对话框的过滤器集合

    private bool addToMruList = true; // 定义字段，用于指示是否将对话框项添加到最近使用列表，初始化为 true

    private bool allowPropertyEditing; // 定义字段，用于指示是否允许编辑属性

    private bool? canceled; // 定义字段，用于指示对话框是否被取消，值为可空布尔型

    private Guid cookieIdentifier; // 定义字段，用于存储对话框的唯一标识符

    private IFileDialogCustomize customize; // 定义字段，用于存储自定义对话框的接口实例

    private string defaultDirectory; // 定义字段，用于存储默认目录的路径

    private ShellContainer defaultDirectoryShellContainer; // 定义字段，用于存储默认目录的 Shell 容器

    private bool ensureFileExists; // 定义字段，用于指示是否确保文件存在

    private bool ensurePathExists; // 定义字段，用于指示是否确保路径存在

    private bool ensureReadOnly; // 定义字段，用于指示是否确保文件为只读

    private bool ensureValidNames; // 定义字段，用于指示是否确保文件名有效

    private bool filterSet; // 定义字段，用于指示是否设置了过滤器

    private string initialDirectory; // 定义字段，用于存储初始目录的路径

    private ShellContainer initialDirectoryShellContainer; // 定义字段，用于存储初始目录的 Shell 容器

    private IFileDialog nativeDialog; // 定义字段，用于存储原生对话框的接口实例

    private NativeDialogEventSink nativeEventSink; // 定义字段，用于存储原生对话框事件的处理实例

    private bool navigateToShortcut = true; // 定义字段，用于指示是否导航到快捷方式，初始化为 true

    private nint parentWindow = 0; // 定义字段，用于存储父窗口的句柄，初始化为 0

    private bool resetSelections; // 定义字段，用于指示是否重置选择

    private bool restoreDirectory; // 定义字段，用于指示是否恢复目录

    private bool showHiddenItems; // 定义字段，用于指示是否显示隐藏项

    private bool showPlacesList = true; // 定义字段，用于指示是否显示位置列表，初始化为 true

    private string title; // 定义字段，用于存储对话框标题

    protected CommonFileDialog() // 构造函数，用于初始化 CommonFileDialog 类的实例
    {
        if (!OsVersionHelper.IsWindowsVista_OrGreater) // 检查操作系统版本是否大于等于 Windows Vista
        {
            throw new PlatformNotSupportedException(LocalizedMessages.CommonFileDialogRequiresVista); // 如果不满足条件，抛出平台不支持异常
        }

        filenames = new Collection<string>(); // 初始化文件名集合
        filters = new CommonFileDialogFilterCollection(); // 初始化过滤器集合
        items = new Collection<IShellItem>(); // 初始化 Shell 项集合
        controls = new CommonFileDialogControlCollection<CommonFileDialogControl>(this); // 初始化控件集合
    }

    protected CommonFileDialog(string title) // 构造函数，接受对话框标题作为参数
        : this() => this.title = title; // 调用无参数的构造函数并设置标题

    public event EventHandler DialogOpening; // 定义 DialogOpening 事件，表示对话框即将打开

    public event CancelEventHandler FileOk; // 定义 FileOk 事件，表示用户确认文件选择

    public event EventHandler FileTypeChanged; // 定义 FileTypeChanged 事件，表示文件类型发生变化

    public event EventHandler FolderChanged; // 定义 FolderChanged 事件，表示文件夹发生变化

    public event EventHandler<CommonFileDialogFolderChangeEventArgs> FolderChanging; // 定义 FolderChanging 事件，表示文件夹正在发生变化，并传递事件参数

    public event EventHandler SelectionChanged; // 定义 SelectionChanged 事件，表示选择发生变化

    public static bool IsPlatformSupported => // 定义静态属性，用于检查平台是否受支持
            OsVersionHelper.IsWindowsVista_OrGreater; // 返回操作系统版本是否大于等于 Windows Vista

    public bool AddToMostRecentlyUsedList // 定义属性，用于获取或设置是否将对话框项添加到最近使用列表
    {
        get => addToMruList; // 获取 addToMruList 字段的值
        set // 设置 addToMruList 字段的值
        {
            ThrowIfDialogShowing(LocalizedMessages.AddToMostRecentlyUsedListCannotBeChanged); // 如果对话框正在显示，抛出异常
            addToMruList = value; // 设置 addToMruList 字段的值
        }
    }

    public bool AllowPropertyEditing // 定义属性，用于获取或设置是否允许编辑属性
    # 获取允许属性编辑的状态
    get => allowPropertyEditing;
    # 设置允许属性编辑的状态
    set => allowPropertyEditing = value;
    }

    # 获取控件集合
    public CommonFileDialogControlCollection<CommonFileDialogControl> Controls => controls;

    # 获取或设置 Cookie 标识符
    public Guid CookieIdentifier
    {
        # 获取 Cookie 标识符
        get => cookieIdentifier;
        # 设置 Cookie 标识符
        set => cookieIdentifier = value;
    }

    # 获取或设置默认目录
    public string DefaultDirectory
    {
        # 获取默认目录
        get => defaultDirectory;
        # 设置默认目录
        set => defaultDirectory = value;
    }

    # 获取或设置默认目录的 Shell 容器
    public ShellContainer DefaultDirectoryShellContainer
    {
        # 获取默认目录的 Shell 容器
        get => defaultDirectoryShellContainer;
        # 设置默认目录的 Shell 容器
        set => defaultDirectoryShellContainer = value;
    }

    # 获取或设置默认扩展名
    public string DefaultExtension { get; set; }

    # 获取或设置默认文件名，初始化为空字符串
    public string DefaultFileName { get; set; } = string.Empty;

    # 获取或设置确保文件存在的标志
    public bool EnsureFileExists
    {
        # 获取确保文件存在的标志
        get => ensureFileExists;
        # 设置确保文件存在的标志，且对话框正在显示时抛出异常
        set
        {
            ThrowIfDialogShowing(LocalizedMessages.EnsureFileExistsCannotBeChanged);
            ensureFileExists = value;
        }
    }

    # 获取或设置确保路径存在的标志
    public bool EnsurePathExists
    {
        # 获取确保路径存在的标志
        get => ensurePathExists;
        # 设置确保路径存在的标志，且对话框正在显示时抛出异常
        set
        {
            ThrowIfDialogShowing(LocalizedMessages.EnsurePathExistsCannotBeChanged);
            ensurePathExists = value;
        }
    }

    # 获取或设置确保只读的标志
    public bool EnsureReadOnly
    {
        # 获取确保只读的标志
        get => ensureReadOnly;
        # 设置确保只读的标志，且对话框正在显示时抛出异常
        set
        {
            ThrowIfDialogShowing(LocalizedMessages.EnsureReadonlyCannotBeChanged);
            ensureReadOnly = value;
        }
    }

    # 获取或设置确保名称有效的标志
    public bool EnsureValidNames
    {
        # 获取确保名称有效的标志
        get => ensureValidNames;
        # 设置确保名称有效的标志，且对话框正在显示时抛出异常
        set
        {
            ThrowIfDialogShowing(LocalizedMessages.EnsureValidNamesCannotBeChanged);
            ensureValidNames = value;
        }
    }

    # 获取文件作为 Shell 对象
    public ShellObject FileAsShellObject
    {
        get
        {
            # 检查文件项是否可用
            CheckFileItemsAvailable();

            # 如果文件项数量大于1，抛出异常
            if (items.Count > 1)
            {
                throw new InvalidOperationException(LocalizedMessages.CommonFileDialogMultipleItems);
            }

            # 如果没有文件项，返回 null
            if (items.Count == 0) { return null!; }

            # 创建并返回第一个 Shell 对象
            return ShellObjectFactory.Create(items[0]);
        }
    }

    # 获取文件名
    public string FileName
    {
        get
        {
            # 检查文件名是否可用
            CheckFileNamesAvailable();

            # 如果文件名数量大于1，抛出异常
            if (filenames.Count > 1)
            {
                throw new InvalidOperationException(LocalizedMessages.CommonFileDialogMultipleFiles);
            }

            # 获取第一个文件名
            var returnFilename = filenames[0];

            # 如果是 CommonSaveFileDialog，修改文件扩展名
            if (this is CommonSaveFileDialog)
            {
                returnFilename = System.IO.Path.ChangeExtension(returnFilename, this.filters[this.SelectedFileTypeIndex - 1].Extensions[0]);
            }

            # 如果默认扩展名不为空，修改文件扩展名
            if (!string.IsNullOrEmpty(DefaultExtension))
            {
                returnFilename = System.IO.Path.ChangeExtension(returnFilename, DefaultExtension);
            }

            # 返回文件名
            return returnFilename;
        }
    }

    # 获取过滤器集合
    public CommonFileDialogFilterCollection Filters => filters;

    # 获取或设置初始目录
    public string InitialDirectory
    {
        # 获取初始目录
        get => initialDirectory;
        # 设置初始目录
        set => initialDirectory = value;
    }
    # 属性：初始目录的 Shell 容器
    public ShellContainer InitialDirectoryShellContainer
    {
        # 获取当前的初始目录 Shell 容器
        get => initialDirectoryShellContainer;
        # 设置新的初始目录 Shell 容器
        set => initialDirectoryShellContainer = value;
    }

    # 属性：是否导航到快捷方式
    public bool NavigateToShortcut
    {
        # 获取当前是否导航到快捷方式的设置
        get => navigateToShortcut;
        # 设置是否导航到快捷方式的设置，并在设置时检查对话框是否正在显示
        set
        {
            ThrowIfDialogShowing(LocalizedMessages.NavigateToShortcutCannotBeChanged);
            navigateToShortcut = value;
        }
    }

    # 属性：是否恢复目录
    public bool RestoreDirectory
    {
        # 获取当前是否恢复目录的设置
        get => restoreDirectory;
        # 设置是否恢复目录的设置，并在设置时检查对话框是否正在显示
        set
        {
            ThrowIfDialogShowing(LocalizedMessages.RestoreDirectoryCannotBeChanged);
            restoreDirectory = value;
        }
    }

    # 属性：当前选择的文件类型索引
    public int SelectedFileTypeIndex
    {
        # 获取当前选择的文件类型索引
        get
        {
            # 如果原生对话框不为空
            if (nativeDialog != null)
            {
                # 获取文件类型索引
                nativeDialog.GetFileTypeIndex(out var fileType);
                return (int)fileType;
            }

            # 如果原生对话框为空，返回 -1
            return -1;
        }
    }

    # 属性：是否显示隐藏项
    public bool ShowHiddenItems
    {
        # 获取当前是否显示隐藏项的设置
        get => showHiddenItems;
        # 设置是否显示隐藏项的设置，并在设置时检查对话框是否正在显示
        set
        {
            ThrowIfDialogShowing(LocalizedMessages.ShowHiddenItemsCannotBeChanged);
            showHiddenItems = value;
        }
    }

    # 属性：是否显示位置列表
    public bool ShowPlacesList
    {
        # 获取当前是否显示位置列表的设置
        get => showPlacesList;
        # 设置是否显示位置列表的设置，并在设置时检查对话框是否正在显示
        set
        {
            ThrowIfDialogShowing(LocalizedMessages.ShowPlacesListCannotBeChanged);
            showPlacesList = value;
        }
    }

    # 属性：对话框标题
    public string Title
    {
        # 获取当前对话框标题
        get => title;
        # 设置对话框标题，如果对话框正在显示则更新标题
        set
        {
            title = value;
            if (NativeDialogShowing) { nativeDialog.SetTitle(value); }
        }
    }

    # 保护属性：文件名集合
    protected IEnumerable<string> FileNameCollection
    {
        # 获取所有文件名
        get
        {
            foreach (var name in filenames)
            {
                yield return name;
            }
        }
    }

    # 私有属性：检查原生对话框是否正在显示
    private bool NativeDialogShowing => (nativeDialog != null)
                        && (showState == DialogShowState.Showing || showState == DialogShowState.Closing);

    # 方法：添加位置到对话框
    public void AddPlace(ShellContainer place, FileDialogAddPlaceLocation location)
    {
        # 检查位置是否为空
        if (place == null!)
        {
            throw new ArgumentNullException("place");
        }

        # 如果原生对话框为空，初始化并获取原生对话框
        if (nativeDialog == null)
        {
            InitializeNativeFileDialog();
            nativeDialog = GetNativeFileDialog();
        }
        # 将位置添加到原生对话框
        nativeDialog?.AddPlace(place.NativeShellItem, (FileDialogAddPlacement)location);
    }

    # 方法：添加位置到对话框（路径版本）
    public void AddPlace(string path, FileDialogAddPlaceLocation location)
    # 如果路径为空或 null，抛出一个参数为空异常
    if (string.IsNullOrEmpty(path)) { throw new ArgumentNullException("path"); }

    # 如果本地对话框尚未初始化
    if (nativeDialog == null)
    {
        # 初始化本地文件对话框
        InitializeNativeFileDialog();
        # 获取本地文件对话框实例
        nativeDialog = GetNativeFileDialog();
    }

    # 创建一个 GUID 实例，表示 IShellItem2 接口
    var guid = new Guid(ShellIIDGuid.IShellItem2);
    # 从路径创建一个 IShellItem2 实例
    var retCode = Shell32.SHCreateItemFromParsingName(path, 0, ref guid, out
    IShellItem2 nativeShellItem);

    # 如果创建 IShellItem2 实例失败
    if (!CoreErrorHelper.Succeeded(retCode))
    {
        # 抛出一个通用控件异常，并包含错误信息
        throw new CommonControlException(LocalizedMessages.CommonFileDialogCannotCreateShellItem, Marshal.GetExceptionForHR(retCode));
    }

    # 如果本地对话框不为 null，添加 IShellItem2 实例到对话框的指定位置
    nativeDialog?.AddPlace(nativeShellItem, (FileDialogAddPlacement)location);
    }

    # 应用集合更改的方法
    public virtual void ApplyCollectionChanged()
    {
        # 获取自定义文件对话框
        GetCustomizedFileDialog();
        # 遍历所有控件
        foreach (var control in controls)
        {
            # 如果控件尚未添加
            if (!control.IsAdded)
            {
                # 设置控件的托管对话框为当前对话框
                control.HostingDialog = this;
                # 关联自定义设置
                control.Attach(customize);
                # 标记控件已被添加
                control.IsAdded = true;
            }
        }
    }

    # 应用控件属性更改的方法
    public virtual void ApplyControlPropertyChange(string propertyName, DialogControl control)
    {
        # 检查 control 是否为空，如果为空，则抛出 ArgumentNullException 异常
        if (control == null)
        {
            throw new ArgumentNullException("control");
        }

        # 声明 CommonFileDialogControl 类型的变量 dialogControl
        CommonFileDialogControl dialogControl;
        # 根据 propertyName 判断属性类型，并处理不同类型的控件
        if (propertyName == "Text")
        {
            # 如果 control 是 CommonFileDialogTextBox 类型，设置其文本
            if (control is CommonFileDialogTextBox textBox)
            {
                customize.SetEditBoxText(control.Id, textBox.Text);
            }
            # 如果 control 是 CommonFileDialogLabel 类型，设置其标签文本
            else if (control is CommonFileDialogLabel label)
            {
                customize.SetControlLabel(control.Id, label.Text);
            }
        }
        # 如果 propertyName 是 "Visible"，且 control 可以转换为 CommonFileDialogControl 类型
        else if (propertyName == "Visible" && (dialogControl = (control as CommonFileDialogControl)!) != null)
        {
            # 获取 control 的当前状态
            customize.GetControlState(control.Id, out var state);

            # 根据 dialogControl 的 Visible 属性设置状态
            if (dialogControl.Visible == true)
            {
                state |= ControlState.Visible;
            }
            else if (dialogControl.Visible == false)
            {
                state &= ~ControlState.Visible;
            }

            # 更新 control 的状态
            customize.SetControlState(control.Id, state);
        }
        # 如果 propertyName 是 "Enabled"，且 control 可以转换为 CommonFileDialogControl 类型
        else if (propertyName == "Enabled" && (dialogControl = (control as CommonFileDialogControl)!) != null)
        {
            # 获取 control 的当前状态
            customize.GetControlState(control.Id, out var state);

            # 根据 dialogControl 的 Enabled 属性设置状态
            if (dialogControl.Enabled == true)
            {
                state |= ControlState.Enable;
            }
            else if (dialogControl.Enabled == false)
            {
                state &= ~ControlState.Enable;
            }

            # 更新 control 的状态
            customize.SetControlState(control.Id, state);
        }
        # 如果 propertyName 是 "SelectedIndex"
        else if (propertyName == "SelectedIndex")
        {
            # 声明 CommonFileDialogRadioButtonList 和 CommonFileDialogComboBox 类型的变量
            CommonFileDialogRadioButtonList list;
            CommonFileDialogComboBox box;

            # 如果 control 是 CommonFileDialogRadioButtonList 类型，设置选中项
            if ((list = (control as CommonFileDialogRadioButtonList)!) != null)
            {
                customize.SetSelectedControlItem(list.Id, list.SelectedIndex);
            }
            # 如果 control 是 CommonFileDialogComboBox 类型，设置选中项
            else if ((box = (control as CommonFileDialogComboBox)!) != null)
            {
                customize.SetSelectedControlItem(box.Id, box.SelectedIndex);
            }
        }
        # 如果 propertyName 是 "IsChecked"
        else if (propertyName == "IsChecked")
        {
            # 如果 control 是 CommonFileDialogCheckBox 类型，设置复选框状态
            if (control is CommonFileDialogCheckBox checkBox)
            {
                customize.SetCheckButtonState(checkBox.Id, checkBox.IsChecked);
            }
        }
    }

    # 释放资源并禁止最终化
    public void Dispose()
    {
        Dispose(true);
        GC.SuppressFinalize(this);
    }

    # 允许集合更改
    public virtual bool IsCollectionChangeAllowed() => true;

    # 控件属性更改是否允许，默认实现抛出未实现异常
    public virtual bool IsControlPropertyChangeAllowed(string propertyName, DialogControl control)
    {
        CommonFileDialog.GenerateNotImplementedException();
        return false;
    }

    # 重置用户选择
    public void ResetUserSelections() => resetSelections = true;

    # 显示对话框
    public CommonFileDialogResult ShowDialog(nint ownerWindowHandle)
    {
        // 检查所有者窗口句柄是否为 0，如果是则抛出异常
        if (ownerWindowHandle == 0)
        {
            // 抛出参数异常，指明所有者窗口句柄无效
            throw new ArgumentException(LocalizedMessages.CommonFileDialogInvalidHandle, "ownerWindowHandle");
        }

        // 将所有者窗口句柄赋值给父窗口变量
        parentWindow = ownerWindowHandle;

        // 显示对话框并返回结果
        return ShowDialog();
    }

    // 根据传入的窗口对象显示对话框
    public CommonFileDialogResult ShowDialog(Window window)
    {
        // 检查传入的窗口对象是否为 null，如是则抛出异常
        if (window == null)
        {
            // 抛出参数为空异常，指明窗口对象不能为空
            throw new ArgumentNullException("window");
        }

        // 获取窗口的互操作句柄并存储在父窗口变量中
        parentWindow = (new WindowInteropHelper(window)).Handle;

        // 显示对话框并返回结果
        return ShowDialog();
    }

    // 显示对话框并返回结果
    public CommonFileDialogResult ShowDialog()
    {
        // 定义结果变量，用于存储对话框的返回结果
        CommonFileDialogResult result;

        // 初始化本地文件对话框
        InitializeNativeFileDialog();
        // 获取本地文件对话框实例
        nativeDialog = GetNativeFileDialog();

        // 应用本地设置到对话框
        ApplyNativeSettings(nativeDialog);
        // 初始化事件处理
        InitializeEventSink(nativeDialog);

        // 如果重置选择状态为真，则将其重置为假
        if (resetSelections)
        {
            resetSelections = false;
        }

        // 设置对话框状态为显示中
        showState = DialogShowState.Showing;
        // 显示对话框并获取返回结果
        var hresult = nativeDialog.Show(parentWindow);
        // 设置对话框状态为关闭
        showState = DialogShowState.Closed;

        // 检查对话框返回的 HRESULT 是否表示用户取消
        if (CoreErrorHelper.Matches(hresult, (int)HResult.Win32ErrorCanceled))
        {
            // 如果用户取消，设置取消标志并返回取消结果
            canceled = true;
            result = CommonFileDialogResult.Cancel;
            // 清空文件名列表
            filenames.Clear();
        }
        else
        {
            // 用户没有取消，设置取消标志为 false，并返回 OK 结果
            canceled = false;
            result = CommonFileDialogResult.Ok;

            // 用文件名填充文件名集合
            PopulateWithFileNames(filenames);

            // 用 IShellItems 填充项集合
            PopulateWithIShellItems(items);
        }

        // 返回对话框结果
        return result;
    }

    // 从 IShellItem 获取文件名
    internal static string GetFileNameFromShellItem(IShellItem item)
    {
        // 初始化文件名变量
        string filename = null!;
        // 获取显示名称的 HRESULT 值
        var hr = item.GetDisplayName(ShellItemDesignNameOptions.DesktopAbsoluteParsing, out var pszString);
        // 如果获取成功且字符串指针不为 0
        if (hr == HResult.Ok && pszString != 0)
        {
            // 将指针转换为字符串并赋值给文件名
            filename = Marshal.PtrToStringAuto(pszString);
            // 释放字符串指针的内存
            Marshal.FreeCoTaskMem(pszString);
        }
        // 返回文件名
        return filename;
    }

    // 获取 IShellItemArray 中指定索引的 IShellItem
    internal static IShellItem GetShellItemAt(IShellItemArray array, int i)
    {
        // 将索引转换为无符号整数
        var index = (uint)i;
        // 获取指定索引的 IShellItem
        array.GetItemAt(index, out var result);
        // 返回获取到的 IShellItem
        return result;
    }

    // 定义清理本地文件对话框的抽象方法
    internal abstract void CleanUpNativeFileDialog();

    // 定义获取派生选项标志的抽象方法
    internal abstract FileOpenOptions GetDerivedOptionFlags(FileOpenOptions flags);

    // 定义获取本地文件对话框的抽象方法
    internal abstract IFileDialog GetNativeFileDialog();

    // 定义初始化本地文件对话框的抽象方法
    internal abstract void InitializeNativeFileDialog();

    // 定义填充文件名集合的抽象方法
    internal abstract void PopulateWithFileNames(Collection<string> names);

    // 定义填充 IShellItems 集合的抽象方法
    internal abstract void PopulateWithIShellItems(Collection<IShellItem> shellItems);

    // 检查文件项是否可用
    protected void CheckFileItemsAvailable()
    {
        // 如果对话框状态不是已关闭，则抛出异常
        if (showState != DialogShowState.Closed)
        {
            // 抛出无效操作异常，指明对话框尚未关闭
            throw new InvalidOperationException(LocalizedMessages.CommonFileDialogNotClosed);
        }

        // 如果对话框被取消，则抛出异常
        if (canceled.GetValueOrDefault())
        {
            // 抛出无效操作异常，指明对话框已被取消
            throw new InvalidOperationException(LocalizedMessages.CommonFileDialogCanceled);
        }

        // 断言文件项列表不为空，防止因对话框被取消或未显示而导致的无效状态
        Debug.Assert(items.Count != 0,
          "Items list empty - shouldn't happen unless dialog canceled or not yet shown.");
    }

    # 检查文件名是否可用
    protected void CheckFileNamesAvailable()
    {
        # 如果对话框未关闭，抛出异常
        if (showState != DialogShowState.Closed)
        {
            throw new InvalidOperationException(LocalizedMessages.CommonFileDialogNotClosed);
        }

        # 如果对话框已取消，抛出异常
        if (canceled.GetValueOrDefault())
        {
            throw new InvalidOperationException(LocalizedMessages.CommonFileDialogCanceled);
        }

        # 确保文件名集合不为空，调试时检查
        Debug.Assert(filenames.Count != 0,
          "FileNames empty - shouldn't happen unless dialog canceled or not yet shown.");
    }

    # 释放资源的虚方法，提供资源清理的实现
    protected virtual void Dispose(bool disposing)
    {
        # 如果正在释放，清理原生文件对话框
        if (disposing)
        {
            CleanUpNativeFileDialog();
        }
    }

    # 文件确认事件的虚方法，触发 FileOk 事件
    protected virtual void OnFileOk(CancelEventArgs e)
    {
        FileOk?.Invoke(this, e);
    }

    # 文件类型变化事件的虚方法，触发 FileTypeChanged 事件
    protected virtual void OnFileTypeChanged(EventArgs e)
    {
        FileTypeChanged?.Invoke(this, e);
    }

    # 文件夹变化事件的虚方法，触发 FolderChanged 事件
    protected virtual void OnFolderChanged(EventArgs e)
    {
        FolderChanged?.Invoke(this, e);
    }

    # 文件夹变化中事件的虚方法，触发 FolderChanging 事件
    protected virtual void OnFolderChanging(CommonFileDialogFolderChangeEventArgs e)
    {
        FolderChanging?.Invoke(this, e);
    }

    # 对话框打开事件的虚方法，触发 DialogOpening 事件
    protected virtual void OnOpening(EventArgs e)
    {
        DialogOpening?.Invoke(this, e);
    }

    # 选择变化事件的虚方法，触发 SelectionChanged 事件
    protected virtual void OnSelectionChanged(EventArgs e)
    {
        SelectionChanged?.Invoke(this, e);
    }

    # 如果对话框正在显示，抛出异常
    protected void ThrowIfDialogShowing(string message)
    {
        if (NativeDialogShowing)
        {
            throw new InvalidOperationException(message);
        }
    }

    # 生成未实现异常的静态方法
    private static void GenerateNotImplementedException() => throw new NotImplementedException(LocalizedMessages.NotImplementedException);

    # 应用原生设置的私有方法
    private void ApplyNativeSettings(IFileDialog dialog)
    # 断言 dialog 对象不为空，否则抛出异常
    Debug.Assert(dialog != null, "No dialog instance to configure");

    # 如果 parentWindow 未设置
    if (parentWindow == 0)
    {
        # 如果当前应用程序和主窗口不为空
        if (Application.Current != null && Application.Current.MainWindow != null)
        {
            # 获取主窗口的句柄
            parentWindow = (new WindowInteropHelper(Application.Current.MainWindow)).Handle;
        }
        # 否则，如果有打开的窗体
        else if (System.Windows.Forms.Application.OpenForms.Count > 0)
        {
            # 获取第一个打开窗体的句柄
            parentWindow = System.Windows.Forms.Application.OpenForms[0].Handle;
        }
    }

    # 创建 IShellItem2 GUID
    var guid = new Guid(ShellIIDGuid.IShellItem2);

    # 设置对话框的选项
    dialog!.SetOptions(CalculateNativeDialogOptionFlags());

    # 如果标题不为空，则设置对话框标题
    if (title != null) { dialog.SetTitle(title); }

    # 如果初始目录 shell 容器不为空，则设置对话框文件夹
    if (initialDirectoryShellContainer != null!)
    {
        dialog.SetFolder(initialDirectoryShellContainer.NativeShellItem);
    }

    # 如果默认目录 shell 容器不为空，则设置对话框默认文件夹
    if (defaultDirectoryShellContainer != null!)
    {
        dialog.SetDefaultFolder(defaultDirectoryShellContainer.NativeShellItem);
    }

    # 如果初始目录不为空，则创建 shell 项并设置对话框文件夹
    if (!string.IsNullOrEmpty(initialDirectory))
    {
        Shell32.SHCreateItemFromParsingName(initialDirectory, 0, ref guid, out
        IShellItem2 initialDirectoryShellItem);

        if (initialDirectoryShellItem != null)
            dialog.SetFolder(initialDirectoryShellItem);
    }

    # 如果默认目录不为空，则创建 shell 项并设置对话框默认文件夹
    if (!string.IsNullOrEmpty(defaultDirectory))
    {
        Shell32.SHCreateItemFromParsingName(defaultDirectory, 0, ref guid, out
        IShellItem2 defaultDirectoryShellItem);

        if (defaultDirectoryShellItem != null)
        {
            dialog.SetDefaultFolder(defaultDirectoryShellItem);
        }
    }

    # 如果有过滤器且过滤器未设置
    if (filters.Count > 0 && !filterSet)
    {
        # 设置文件类型过滤器
        dialog.SetFileTypes(
            (uint)filters.Count,
            filters.GetAllFilterSpecs());

        # 标记过滤器已设置
        filterSet = true;

        # 同步文件类型组合框到默认扩展名
        SyncFileTypeComboToDefaultExtension(dialog);
    }

    # 如果 cookieIdentifier 不为空，设置客户端 GUID
    if (cookieIdentifier != Guid.Empty)
    {
        dialog.SetClientGuid(ref cookieIdentifier);
    }

    # 如果默认扩展名不为空，则设置对话框默认扩展名
    if (!string.IsNullOrEmpty(DefaultExtension))
    {
        dialog.SetDefaultExtension(DefaultExtension);
    }

    # 设置对话框文件名
    dialog.SetFileName(DefaultFileName);
    {
        # 初始化文件打开选项，不测试文件创建
        var flags = FileOpenOptions.NoTestFileCreate;

        # 获取派生的选项标志
        flags = GetDerivedOptionFlags(flags);

        # 如果需要确保文件存在，则添加 FileMustExist 标志
        if (ensureFileExists)
        {
            flags |= FileOpenOptions.FileMustExist;
        }
        # 如果需要确保路径存在，则添加 PathMustExist 标志
        if (ensurePathExists)
        {
            flags |= FileOpenOptions.PathMustExist;
        }
        # 如果不需要验证名称，则添加 NoValidate 标志
        if (!ensureValidNames)
        {
            flags |= FileOpenOptions.NoValidate;
        }
        # 如果不需要只读模式，则添加 NoReadOnlyReturn 标志
        if (!EnsureReadOnly)
        {
            flags |= FileOpenOptions.NoReadOnlyReturn;
        }
        # 如果需要恢复目录，则添加 NoChangeDirectory 标志
        if (restoreDirectory)
        {
            flags |= FileOpenOptions.NoChangeDirectory;
        }
        # 如果不需要显示位置列表，则添加 HidePinnedPlaces 标志
        if (!showPlacesList)
        {
            flags |= FileOpenOptions.HidePinnedPlaces;
        }
        # 如果不需要添加到最近使用列表，则添加 DontAddToRecent 标志
        if (!addToMruList)
        {
            flags |= FileOpenOptions.DontAddToRecent;
        }
        # 如果需要显示隐藏项，则添加 ForceShowHidden 标志
        if (showHiddenItems)
        {
            flags |= FileOpenOptions.ForceShowHidden;
        }
        # 如果不需要导航到快捷方式，则添加 NoDereferenceLinks 标志
        if (!navigateToShortcut)
        {
            flags |= FileOpenOptions.NoDereferenceLinks;
        }
        # 返回最终的选项标志
        return flags;
    }

    # 获取自定义的文件对话框
    private void GetCustomizedFileDialog()
    {
        # 如果自定义对话框为 null，则初始化原生文件对话框
        if (customize == null)
        {
            # 如果原生对话框也为 null，则初始化并获取原生文件对话框
            if (nativeDialog == null)
            {
                InitializeNativeFileDialog();
                nativeDialog = GetNativeFileDialog();
            }
            # 将原生对话框转换为 IFileDialogCustomize 接口
            customize = (IFileDialogCustomize)nativeDialog;
        }
    }

    # 初始化事件接收器
    private void InitializeEventSink(IFileDialog nativeDlg)
    {
        # 如果有任何事件处理程序或控件，则初始化事件接收器
        if (FileOk != null
            || FolderChanging != null
            || FolderChanged != null
            || SelectionChanged != null
            || FileTypeChanged != null
            || DialogOpening != null
            || (controls != null && controls.Count > 0))
        {
            # 创建事件接收器实例
            nativeEventSink = new NativeDialogEventSink(this);
            # 将事件接收器与原生对话框关联
            nativeDlg.Advise(nativeEventSink, out var cookie);
            # 保存事件接收器的 cookie
            nativeEventSink.Cookie = cookie;
        }
    }

    # 将文件类型组合框同步到默认扩展名
    private void SyncFileTypeComboToDefaultExtension(IFileDialog dialog)
    {
        # 如果不是 CommonSaveFileDialog 或默认扩展名为 null 或过滤器数量为零，则直接返回
        if (this is not CommonSaveFileDialog || DefaultExtension == null ||
            filters.Count <= 0)
        {
            return;
        }

        CommonFileDialogFilter filter;

        # 遍历所有过滤器，找到包含默认扩展名的过滤器
        for (uint filtersCounter = 0; filtersCounter < filters.Count; filtersCounter++)
        {
            filter = filters[(int)filtersCounter];

            # 如果过滤器的扩展名包含默认扩展名，则设置文件类型索引
            if (filter.Extensions.Contains(DefaultExtension))
            {
                dialog.SetFileTypeIndex(filtersCounter + 1);
                break;
            }
        }
    }

    # 内部类，处理文件对话框事件
    private class NativeDialogEventSink : IFileDialogEvents, IFileDialogControlEvents
    }
请提供需要注释的代码部分，我会根据示例为每行代码添加注释。
```