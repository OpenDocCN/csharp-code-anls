# `.\better-genshin-impact\Build\MicaSetup\Natives\Shell\Dialogs\LocalizedMessages.cs`

```cs
namespace MicaSetup.Shell.Dialogs; // 定义命名空间 MicaSetup.Shell.Dialogs

public static class LocalizedMessages // 定义一个公开的静态类 LocalizedMessages
{
    // 定义常量字符串，表示最大允许的按钮数量是 7
    public const string ThumbnailToolbarManagerMaxButtons = "Maximum number of buttons allowed is 7.";
    // 定义常量字符串，表示未知的对话框控制类型
    public const string TaskDialogUnkownControl = "Unknown dialog control type.";
    // 定义常量字符串，表示无效的 CanonicalName 索引
    public const string PropertyCollectionCanonicalInvalidIndex = "This CanonicalName is not a valid index.";
    // 定义常量字符串，表示提供的最小值必须是正数
    public const string TaskDialogProgressBarMinValueGreaterThanZero = "Minimum value provided must be a positive number.";
    // 定义常量字符串，表示对话框被取消，文件名不可用
    public const string CommonFileDialogCanceled = "File name not available - dialog was canceled.";
    // 定义常量字符串，表示方法或操作尚未实现
    public const string NotImplementedException = "The method or operation is not implemented.";
    // 定义常量字符串，表示控件名称不能为 null 或零长度
    public const string DialogCollectionControlNameNull = "Control name cannot be null or zero length.";
    // 定义常量字符串，表示无法获取图标大小
    public const string ExplorerBrowserIconSize = "Unable to get icon size.";
    // 定义常量字符串，表示方法必须从注册的回调方法中调用
    public const string ApplicationRecoveryMustBeCalledFromCallback = "This method must be called from the registered callback method.";
    // 定义常量字符串，表示不能有多个同类型的默认按钮
    public const string TaskDialogOnlyOneDefaultControl = "Cannot have more than one default button of a given type.";
    // 定义常量字符串，表示没有注册处理该值的监听器
    public const string MessageListenerFilterUnknownListenerHandle = "No listener handled of that value is registered.";
    // 定义常量字符串，表示不支持多维 SafeArrays
    public const string PropVariantMultiDimArray = "Multi-dimensional SafeArrays not supported.";
    // 定义常量字符串，表示子控件的窗口句柄不能为零
    public const string TabbedThumbnailZeroChildHandle = "Child control's window handle cannot be zero.";
    // 定义常量字符串，表示无法设置排序列
    public const string ShellSearchFolderUnableToSetSortColumns = "Unable to set list of sort columns.";
    // 定义常量字符串，表示文件夹类型为图片
    public const string FolderTypePictures = "Pictures";
    // 定义常量字符串，表示 TaskDialog 特性需要加载版本 6 的 comctl32.dll，但当前加载的版本不同
    public const string NativeTaskDialogVersionError = "TaskDialog feature needs to load version 6 of comctl32.dll but a different version is current loaded in memory.";
    // 定义常量字符串，表示对话框显示时无法更改 CheckBox 文本
    public const string CheckBoxCannotBeChanged = "CheckBox text cannot be changed while dialog is showing.";
    // 定义常量字符串，表示文件夹类型为 RecordedTV
    public const string FolderTypeRecordedTV = "RecordedTV";
    // 定义常量字符串，表示关闭最近文档跟踪时无法添加自定义类别
    public const string JumpListCustomCategoriesDisabled = "Custom categories cannot be added while recent documents tracking is turned off.";
    // 定义常量字符串，表示提供的句柄不能为 0
    public const string CommonFileDialogInvalidHandle = "Handle provided cannot be 0.";
    // 定义常量字符串，表示对话框中不能有多个相同名称的控件
    public const string DialogControlCollectionMoreThanOneControl = "Dialog cannot have more than one control with the same name.";
    // 定义常量字符串，表示获取活动电源方案失败
    public const string PowerManagerActiveSchemeFailed = "Failed to get active power scheme.";
    // 定义常量字符串，表示对话框显示时不能更改标题
    public const string CaptionCannotBeChanged = "Dialog caption cannot be changed while dialog is showing.";
    // 定义常量字符串，表示对话框显示时不能更改覆盖提示
    public const string OverwritePromptCannotBeChanged = "OverwritePrompt cannot be changed while dialog is showing.";
    // 定义常量字符串，表示父窗口句柄不能为零
    public const string TabbedThumbnailZeroParentHandle = "Parent window handle cannot be zero.";
    // 定义常量字符串，表示对话框显示时不能更改创建提示
    public const string CreatePromptCannotBeChanged = "CreatePrompt cannot be changed while dialog is showing.";
    // 定义常量字符串，表示文件夹类型为搜索
    public const string FolderTypeSearches = "Searches";
    // 错误信息：无法创建 Shell 项
    public const string ShellObjectCreationFailed = "Shell item could not be created.";
    // 错误信息：自定义控件一旦添加到文件对话框中，不能被移除
    public const string DialogControlCollectionCannotRemoveControls = "Custom controls cannot be removed from a File dialog once added.";
    // 错误信息：此预览已被添加
    public const string ThumbnailManagerPreviewAdded = "This preview has already been added.";
    // 错误信息：标准按钮在对话框显示时不能被更改
    public const string StandardButtonsCannotBeChanged = "StandardButtons cannot be changed while dialog is showing.";
    // 错误信息：对话框不能同时显示非标准按钮和标准按钮
    public const string TaskDialogSupportedButtonsAndButtons = "Dialog cannot display both non-standard buttons and standard buttons.";
    // 错误信息：当前大小（宽度或高度）不能大于最大尺寸：{0}
    public const string ShellThumbnailCurrentSizeRange = "CurrentSize (width or height) cannot be greater than the maximum size: {0}.";
    // 错误信息：检索选择时发生意外错误
    public const string ExplorerBrowserUnexpectedError = "Unexpected error retrieving selection.";
    // 错误信息：检索项目计数时发生意外错误
    public const string ExplorerBrowserItemCount = "Unexpected error retrieving item count.";
    // 错误信息：更改通知注册失败
    public const string ShellObjectWatcherRegisterFailed = "Registration for change notification has failed.";
    // 错误信息：应用程序注册恢复失败
    public const string ApplicationRecoveryFailedToRegister = "Application failed to register for recovery.";
    // 属性键格式字符串
    public const string PropertyKeyFormatString = "{0}, {1}";
    // 错误信息：对话框显示时不能更改 EnsurePathExists
    public const string EnsurePathExistsCannotBeChanged = "EnsurePathExists cannot be changed while dialog is showing.";
    // 错误信息：索引超出了 CommonFileDialogComboBox 的范围
    public const string ComboBoxIndexOutsideBounds = "Index was outside the bounds of the CommonFileDialogComboBox.";
    // 文件夹类型：通用库
    public const string FolderTypeGenericLibrary = "Generic Library";
    // 错误信息：JumpListLink 的标题是必需的，不能为 null
    public const string JumpListLinkTitleRequired = "JumpListLink's title is required and cannot be null.";
    // 错误信息：默认按钮在对话框显示时不能被更改
    public const string DefaultButtonCannotBeChanged = "Default Button cannot be changed while dialog is showing.";
    // 错误信息：参考路径无效
    public const string InvalidReferencePath = "Reference path is invalid.";
    // 文件夹类型：打印机
    public const string FolderTypePrinters = "Printers";
    // 错误信息：当前 ShellObject 没有有效的缩略图处理程序或提取缩略图时出现问题
    public const string ShellThumbnailNoHandler = "The current ShellObject does not have a valid thumbnail handler or there was a problem in extracting the thumbnail for this specific shell object.";
    // 错误信息：提供的值必须大于等于最小值且小于最大值
    public const string TaskDialogProgressBarValueInRange = "Value provided must be greater than equal to the minimum value and less than the maximum value.";
    // 错误信息：应用程序注册重启失败
    public const string ApplicationRecoveryFailedToRegisterForRestart = "Application failed to registered for restart.";
    // 错误信息：无法向只读列表中插入项目
    public const string ShellObjectCollectionInsertReadOnly = "Cannot insert items into a read only list.";
    // 错误信息：无法创建 Shell 项
    public const string ShellObjectFactoryUnableToCreateItem = "Unable to Create Shell Item.";
    // 错误信息：字符串参数不能为 null 或空
    public const string PropVariantNullString = "String argument cannot be null or empty.";
    // 错误信息：该属性仅接受类型为“{0}”的值
    public const string ShellPropertyWrongType = "This property only accepts a value of type \"{0}\".";
    // 错误信息：对话框显示时不能更改 AlwaysAppendDefaultExtension
    public const string AlwaysAppendDefaultExtensionCannotBeChanged = "AlwaysAppendDefaultExtension cannot be changed while dialog is showing.";
    // 错误信息：无效
    public const string FolderTypeInvalid = "Invalid";
    # 定义一个常量字符串，表示在对话框显示时无法更改展开信息模式
    public const string ExpandedDetailsCannotBeChanged = "Expanded information mode cannot be changed while dialog is showing.";
    # 定义一个常量字符串，表示指定的事件处理程序未注册
    public const string MessageManagerHandlerNotRegistered = "The specified event handler has not been registered.";
    # 定义一个常量字符串，表示在对话框显示时无法更改 IsExpandedMode
    public const string IsExpandedModeCannotBeChanged = "IsExpandedMode cannot be changed while dialog is showing.";
    # 定义一个常量字符串，表示在对话框显示时无法更改 AddToMostRecentlyUsedList
    public const string AddToMostRecentlyUsedListCannotBeChanged = "AddToMostRecentlyUsedList cannot be changed while dialog is showing.";
    # 定义一个常量字符串，表示集合必须至少包含一个 shell 对象
    public const string ShellObjectCollectionEmptyCollection = "Must have at least one shell object in the collection.";
    # 定义一个常量字符串，表示调用者没有足够的权限来获取系统电池状态
    public const string PowerInsufficientAccessBatteryState = "The caller had insufficient access rights to get the system battery state.";
    # 定义一个常量字符串，表示无法设置属性
    public const string ShellPropertySetValue = "Unable to set property.";
    # 定义一个常量字符串，表示无法初始化 PropVariant
    public const string PropVariantInitializationError = "Unable to initialize PropVariant.";
    # 定义一个常量字符串，表示用户文件
    public const string FolderTypeUserFiles = "User Files";
    # 定义一个常量字符串，表示给定路径不存在
    public const string FilePathNotExist = "The given path does not exist ({0})";
    # 定义一个常量字符串，表示尝试关闭一个未显示的对话框
    public const string TaskDialogCloseNonShowing = "Attempting to close a non-showing dialog.";
    # 定义一个常量字符串，表示窗口创建失败，需要查看内部异常以获取详细信息
    public const string MessageListenerCannotCreateWindow = "Creation of window has failed, view inner exception for details.";
    # 定义一个常量字符串，表示从对话框检索到一个空的 shell 项
    public const string SaveFileNullItem = "Retrieved a null shell item from dialog.";
    # 定义一个常量字符串，表示在对话框显示时无法更改 NavigateToShortcut
    public const string NavigateToShortcutCannotBeChanged = "NavigateToShortcut cannot be changed while dialog is showing.";
    # 定义一个常量字符串，表示不能从只读列表中移除项目
    public const string ShellObjectCollectionRemoveReadOnly = "Cannot remove items from a read only list.";
    # 定义一个常量字符串，表示字符串中的值被截断或数值被四舍五入。设置 AllowTruncatedValue 为 true 以防止此异常
    public const string ShellPropertyValueTruncated = "A value had to be truncated in a string or rounded if a numeric value. Set AllowTruncatedValue to true to prevent this exception.";
    # 定义一个常量字符串，表示所有图片文件
    public const string CommonFiltersPicture = "All Picture Files";
    # 定义一个常量字符串，表示取消注册失败
    public const string ApplicationRecoveryFailedToUnregisterForRestart = "Unregister for restart failed.";
    # 定义一个常量字符串，表示在对话框显示时无法更改 EnsureFileExists
    public const string EnsureFileExistsCannotBeChanged = "EnsureFileExists cannot be changed while dialog is showing.";
    # 定义一个常量字符串，表示无法设置可见列
    public const string ShellSearchFolderUnableToSetVisibleColumns = "Unable to set visible columns.";
    # 定义一个常量字符串，表示视频
    public const string FolderTypeVideos = "Videos";
    # 定义一个常量字符串，表示 PropertyKey 不是有效的索引
    public const string PropertyCollectionInvalidIndex = "This PropertyKey is not a valid index.";
    # 定义一个常量字符串，表示不允许使用空的或空数组
    public const string ThumbnailToolbarManagerNullEmptyArray = "Null or empty arrays are not allowed.";
    # 定义一个常量字符串，表示给定的控件未添加到任务栏
    public const string ThumbnailManagerControlNotAdded = "The given control has not been added to the taskbar.";
    # 定义一个常量字符串，表示当前大小（宽度或高度）不能为 0
    public const string ShellThumbnailSizeCannotBe0 = "CurrentSize (width or height) cannot be 0.";
    # 定义一个常量字符串，表示给定的已知文件夹不是有效的库
    public const string ShellLibraryInvalidLibrary = "The given known folder is not a valid library.";
    # 定义一个常量字符串，表示获取解析名称失败
    public const string ShellHelperGetParsingNameFailed = "GetParsingName has failed.";
    # 定义一个常量字符串，表示 SetThreadExecutionState 调用失败
    public const string PowerExecutionStateFailed = "SetThreadExecutionState call failed.";
    # 定义消息监听器过滤器注册失败的错误信息
    public const string MessageListenerFilterUnableToRegister = "Message filter registration failed.";
    # 定义文件夹类型为搜索连接器的字符串常量
    public const string FolderTypeSearchConnector = "Search Connector";
    # 定义在对话框显示时无法更改扩展控件标签的错误信息
    public const string ExpandedLabelCannotBeChanged = "Expanded control label cannot be changed while dialog is showing.";
    # 定义文件对话框尚未关闭时，文件名不可用的错误信息
    public const string CommonFileDialogNotClosed = "File name not available - dialog has not closed yet.";
    # 定义文件夹类型为控制面板类别的字符串常量
    public const string FolderTypeCategory = "ControlPanel Category";
    # 定义对话框的默认标题
    public const string DialogDefaultCaption = "Application";
    # 定义浏览对象失败的错误信息
    public const string ExplorerBrowserBrowseToObjectFailed = "Browsing to object failed.";
    # 定义窗口类注册失败的错误信息，需要检查内部异常获取更多细节
    public const string MessageListenerClassNotRegistered = "Window class could not be registered, check inner exception for more details.";
    # 定义文件夹类型为联系人类别的字符串常量
    public const string FolderTypeContacts = "Contacts";
    # 定义任务对话框的默认标题
    public const string TaskDialogDefaultCaption = "Application";
    # 定义任务栏窗口管理器按钮已添加的错误信息，提示查看 AddButtons 方法的备注部分以获取更多信息
    public const string TaskbarWindowManagerButtonsAlreadyAdded = "Tool bar buttons for this window are already added. Please refer to the Remarks section of the AddButtons method for more information on updating the properties or hiding existing buttons.";
    # 定义单选按钮列表索引超出范围的错误信息
    public const string RadioButtonListIndexOutOfBounds = "Index was outside the bounds of the CommonFileDialogRadioButtonList.";
    # 定义在监听时无法更改观察的事件的错误信息
    public const string ShellObjectWatcherUnableToChangeEvents = "Unable to change watched events while listening.";
    # 定义给定的 CanonicalName 无效的错误信息
    public const string ShellInvalidCanonicalName = "The given CanonicalName is not valid.";
    # 定义文件夹类型为压缩文件夹的字符串常量
    public const string FolderTypeCompressedFolder = "Compressed Folder";
    # 定义多选文件对话框中选择多个文件时，应该使用 Items 属性的提示信息
    public const string CommonFileDialogMultipleItems = "Multiple files selected - the Items property should be used instead.";
    # 定义无效的文件夹类型 Guid 的错误信息
    public const string ShellLibraryInvalidFolderType = "Invalid FolderType Guid.";
    # 定义文件夹类型为音乐的字符串常量
    public const string FolderTypeMusic = "Music";
    # 定义任务对话框的默认主指令为空字符串
    public const string TaskDialogDefaultMainInstruction = "";
    # 定义在对话框显示时无法更改可取消属性的错误信息
    public const string CancelableCannotBeChanged = "Cancelable cannot be changed while dialog is showing.";
    # 定义库名称不能为空的错误信息
    public const string ShellLibraryEmptyName = "LibraryName cannot be empty.";
    # 定义对话框的默认主指令为空字符串
    public const string DialogDefaultMainInstruction = "";
    # 定义在对话框显示时无法设置超链接的错误信息
    public const string HyperlinksCannotBetSet = "Hyperlinks cannot be enabled/disabled while dialog is showing.";
    # 定义调用者获取系统电源功能不足的错误信息
    public const string PowerInsufficientAccessCapabilities = "The caller had insufficient access rights to get the system power capabilities.";
    # 定义文件夹类型为开放搜索的字符串常量
    public const string FolderTypeOpenSearch = "Open Search";
    # 定义对话框的默认内容为空字符串
    public const string DialogDefaultContent = "";
    # 定义无法获取显示名称的错误信息
    public const string ShellObjectCannotGetDisplayName = "Can't get the display name.";
    # 定义常见过滤器的文本描述为文本文件
    public const string CommonFiltersText = "Text Files";
    # 定义无法创建 Shell 项的错误信息
    public const string CommonFileDialogCannotCreateShellItem = "Shell item could not be created.";
    # 定义因参数错误而未能注册应用程序重启的错误信息
    public const string ApplicationRecoverFailedToRegisterForRestartBadParameters = "Failed to register application for restart due to bad parameters.";
    # 定义常量，表示对话框控件集合菜单项控制只能添加到 CommonFileDialogMenu 控件
    public const string DialogControlCollectionMenuItemControlsCannotBeAdded = "CommonFileDialogMenuItem controls can only be added to CommonFileDialogMenu controls.";
    # 定义常量，表示文件夹类型为“已保存的游戏”
    public const string FolderTypeSavedGames = "Saved Games";
    # 定义常量，表示按钮文本不能为空
    public const string TaskDialogButtonTextEmpty = "Button text must be non-empty.";
    # 定义常量，表示关闭事件中的按钮 ID 错误
    public const string TaskDialogBadButtonId = "Bad button ID in closing event.";
    # 定义常量，表示任务栏窗口按钮数组必须包含至少一个项目
    public const string TaskbarWindowEmptyButtonArray = "The array of buttons must contain at least 1 item.";
    # 定义常量，表示对话框控件必须先从当前集合中移除
    public const string DialogControlCollectionRemoveControlFirst = "Dialog control must be removed from current collections first.";
    # 定义常量，表示无法获取此属性的可写属性存储
    public const string ShellPropertyUnableToGetWritableProperty = "Unable to get writable property store for this property.";
    # 定义常量，表示文件夹类型为“控制面板经典”
    public const string FolderTypeClassic = "ControlPanel Classic";
    # 定义常量，表示对话框显示时无法更改 EnsureReadOnly 属性
    public const string EnsureReadonlyCannotBeChanged = "EnsureReadOnly cannot be changed while dialog is showing.";
    # 定义常量，表示对话框显示时无法更改展开状态
    public const string ExpandingStateCannotBeChanged = "Expanding state of the dialog cannot be changed while dialog is showing.";
    # 定义常量，表示发生 Shell 异常，查看内部异常以获取信息
    public const string ShellExceptionDefaultText = "Shell Exception has occurred, look at inner exception for information.";
    # 定义常量，表示进度条不能在多个对话框中托管
    public const string ProgressBarCannotBeHostedInMultipleDialogs = "Progress bar cannot be hosted in multiple dialogs.";
    # 定义常量，表示启用对话框复选框时必须提供复选框文本
    public const string TaskDialogCheckBoxTextRequiredToEnableCheckBox = "Check box text must be provided to enable the dialog check box.";
    # 定义常量，表示窗口句柄无效
    public const string ThumbnailManagerInvalidHandle = "Window handle is invalid.";
    # 定义常量，表示控件名称不能为空或长度为零
    public const string DialogControlCollectionEmptyName = "Control name cannot be null or zero length.";
    # 定义常量，表示文件夹类型为“通用搜索结果”
    public const string FolderTypeSearchResults = "Generic SearchResults.";
    # 定义常量，表示此值类型不受支持
    public const string PropVariantTypeNotSupported = "This Value type is not supported.";
    # 定义常量，表示在对话框显示时修改控件集合是不支持的
    public const string DialogControlCollectionModifyingControls = "Modifying controls collection while dialog is showing is not supported.";
    # 定义常量，表示文件夹类型为“游戏”
    public const string FolderTypeGames = "Games.";
    # 定义常量，表示 Common File Dialog 需要 Windows Vista 或更高版本
    public const string CommonFileDialogRequiresVista = "Common File Dialog requires Windows Vista or later.";
    # 定义常量，表示属性集合的 CanonicalName 参数不能为空或为空
    public const string PropertyCollectionNullCanonicalName = "Argument CanonicalName cannot be null or empty.";
    # 定义常量，表示找不到 DefaultSaveFolder 路径
    public const string ShellLibraryDefaultSaveFolderNotFound = "DefaultSaveFolder path not found.";
    # 定义常量，表示恢复设置的格式字符串
    public const string RecoverySettingsFormatString = "delegate: {0}, state: {1}, ping: {2}";
    # 定义常量，表示文件夹类型为“网络浏览器”
    public const string FolderTypeNetworkExplorer = "Network Explorer.";
    # 定义常量，表示对话框显示时无法更改显示位置列表
    public const string ShowPlacesListCannotBeChanged = "Show places list cannot be changed while dialog is showing.";
    # 定义常量，表示检索选定项计数时发生意外错误
    public const string ExplorerBrowserSelectedItemCount = "Unexpected error retrieving selected item count.";
    # 定义常量，表示应用程序因参数错误未注册为恢复应用程序
    public const string ApplicationRecoveryBadParameters = "Application was not registered for recovery due to bad parameters.";
    # 定义常量字符串，用于提示用户该属性的值不能被设置，需要使用相关的 ShellObject 来设置属性值
    public const string ShellPropertyCannotSetProperty = "The value on this property cannot be set. To set the property value, use the ShellObject that is associated with this property.";
    # 定义常量字符串，用于表示文件过滤器类型为 Office 文件
    public const string CommonFiltersOffice = "Office Files";
    # 定义常量字符串，用于提示在对话框显示时，不能更改折叠控件的文本
    public const string CollapsedTextCannotBeChanged = "Collapsed control text cannot be changed while dialog is showing.";
    # 定义常量字符串，用于提示对话框内容过于复杂
    public const string NativeTaskDialogInternalErrorComplex = "Dialog contents too complex.";
    # 定义常量字符串，用于提示 Win32 调用的参数无效
    public const string NativeTaskDialogInternalErrorArgs = "Invalid arguments to Win32 call.";
    # 定义常量字符串，用于提示无法转换为不支持的类型
    public const string PropVariantUnsupportedType = "Cannot be cast to unsupported type.";
    # 定义常量字符串，用于表示文件夹类型为库
    public const string FolderTypeLibrary = "Library";
    # 定义常量字符串，用于提示任务对话框的进度条最大值必须大于最小值
    public const string TaskDialogProgressBarMaxValueGreaterThanMin = "Maximum value provided must be greater than the minimum value.";
    # 定义常量字符串，用于表示文件夹类型为软件资源管理器
    public const string FolderTypeSoftwareExplorer = "Software Explorer";
    # 定义常量字符串，用于提示给定的预览未被添加到任务栏
    public const string ThumbnailManagerPreviewNotAdded = "The given preview has not been added to the taskbar.";
    # 定义常量字符串，用于提示在对话框显示时，不能修改控件集合
    public const string DialogCollectionModifyShowingDialog = "Modifying controls collection while dialog is showing is not supported.";
    # 定义常量字符串，用于表示文件夹类型未指定
    public const string FolderTypeNotSpecified = "Not Specified";
    # 定义常量字符串，用于提示对话框配置中出现错误
    public const string NativeTaskDialogConfigurationError = "An error has occurred in dialog configuration.";
    # 定义常量字符串，用于表示文件夹类型为音乐图标
    public const string FolderTypeMusicIcons = "Music Icons";
    # 定义常量字符串，用于表示文件夹类型为通信
    public const string FolderTypeCommunications = "Communications";
    # 定义常量字符串，用于提示当前 ShellObject 没有缩略图，建议使用 ShellThumbnailFormatOption.Default 来获取该项的图标
    public const string ShellThumbnailDoesNotHaveThumbnail = "The current ShellObject does not have a thumbnail. Try using ShellThumbnailFormatOption.Default to get the icon for this item.";
    # 定义常量字符串，用于提示 ExplorerBrowser 获取当前视图失败
    public const string ExplorerBrowserFailedToGetView = "ExplorerBrowser failed to get current view.";
    # 定义常量字符串，用于任务对话框的默认内容为空
    public const string TaskDialogDefaultContent = "";
    # 定义常量字符串，用于提示解析名称无效
    public const string KnownFolderParsingName = "Parsing name is invalid.";
    # 定义常量字符串，用于提示导航日志的父项不能为空
    public const string NavigationLogNullParent = "Parent cannot be null.";
    # 定义常量字符串，用于提示任务对话框的进度条最小值必须小于最大值
    public const string TaskDialogProgressBarMinValueLessThanMax = "Minimum value provided must less than the maximum value.";
    # 定义常量字符串，用于提示对话框显示时，无法更改对话框拥有者
    public const string OwnerCannotBeChanged = "Dialog owner cannot be changed while dialog is showing.";
    # 定义常量字符串，用于提示对话框不能同时显示非标准按钮和命令链接
    public const string TaskDialogSupportedButtonsAndLinks = "Dialog cannot display both non-standard buttons and command links.";
    # 定义常量字符串，用于提示在对话框显示时，不能更改显示隐藏项目的设置
    public const string ShowHiddenItemsCannotBeChanged = "ShowHiddenItems cannot be changed while dialog is showing.";
    # 定义常量字符串，用于提示对话框控件不能被重命名
    public const string DialogControlsCannotBeRenamed = "Dialog controls cannot be renamed.";
    # 定义常量字符串，用于提示给定的属性键无效
    public const string SearchConditionFactoryInvalidProperty = "Given property key is invalid.";
    # 定义常量字符串，用于提示选择了多个文件时，应使用 FileNames 属性
    public const string CommonFileDialogMultipleFiles = "Multiple files selected - the FileNames property should be used instead.";
    # 定义常量字符串，用于格式化重启设置的命令和限制
    public const string RestartSettingsFormatString = "command: {0} restrictions: {1}";
    # 定义常量字符串，用于提示给定的股票图标标识符无效
    public const string StockIconInvalidGuid = "The Stock Icon identifier given is invalid ({0}).";
    // 表示创建 Shell 对象需要 Windows Vista 或更高版本操作系统的错误信息
    public const string ShellObjectFactoryPlatformNotSupported = "Shell Object creation requires Windows Vista or higher operating system.";
    // 表示 Win32 调用中发生了意外内部错误的错误信息
    public const string NativeTaskDialogInternalErrorUnexpected = "An unexpected internal error occurred in the Win32 call: {0:x}";
    // 表示电池状态的字符串表示形式，包括多个相关电池参数
    public const string BatteryStateStringRepresentation = "ACOnline: {1}{0}Max Charge: {2} mWh{0}Current Charge: {3} mWh{0}Discharge Rate: {4} mWh{0}Estimated Time Remaining: {5}{0}Suggested Critical Battery Charge: {6} mWh{0}Suggested Battery Warning Charge: {7} mWh{0}";
    // 表示此属性仅在 Windows 7 上可用的提示信息
    public const string ShellPropertyWindows7 = "This Property is available on Windows 7 only.";
    // 表示文件夹类型为 "文档"
    public const string FolderTypeDocuments = "Documents";
    // 表示应用程序恢复失败以取消注册的错误信息
    public const string ApplicationRecoveryFailedToUnregister = "Unregister for recovery failed.";
    // 表示任务栏窗口值已设置，不能重复设置的错误信息
    public const string TaskbarWindowValueSet = "Value is already set. It cannot be set more than once.";
    // 表示文件夹类型为 "回收站"
    public const string FolderTypeRecycleBin = "RecycleBin";
    // 表示意外错误检索视图项的错误信息
    public const string ExplorerBrowserViewItems = "Unexpected error retrieving view items.";
    // 表示给定的已知文件夹 ID 无效的错误信息
    public const string KnownFolderInvalidGuid = "Given Known Folder ID is invalid.";
    // 表示在对话框显示时无法更改 RestoreDirectory 的错误信息
    public const string RestoreDirectoryCannotBeChanged = "RestoreDirectory cannot be changed while dialog is showing.";
    // 表示在对话框显示时无法更改 EnsureValidNames 的错误信息
    public const string EnsureValidNamesCannotBeChanged = "EnsureValidNames cannot be changed while dialog is showing.";
    // 表示在对话框显示时无法更改进度条的错误信息
    public const string ProgressBarCannotBeChanged = "Progress bar cannot be changed while dialog is showing.";
    // 表示不允许负数作为序数位置的错误信息
    public const string JumpListNegativeOrdinalPosition = "Negative numbers are not allowed for the ordinal position.";
    // 表示对话框控件必须先从当前集合中移除的错误信息
    public const string DialogCollectionControlAlreadyHosted = "Dialog control must be removed from current collections first.";
    // 表示需要有效的活动窗口来更新任务栏的错误信息
    public const string TaskbarManagerValidWindowRequired = "A valid active Window is needed to update the Taskbar.";
    // 表示文件夹类型为 "用户库"
    public const string FolderTypeUserLibraries = "Users Libraries";
    // 表示文件夹路径未找到的错误信息
    public const string ShellLibraryFolderNotFound = "Folder path not found.";
    // 表示文件夹类型为 "其他用户"
    public const string FolderTypeOtherUsers = "Other Users";
    // 表示 JumpListLink 的路径是必需的，不能为 null 的错误信息
    public const string JumpListLinkPathRequired = "JumpListLink's path is required and cannot be null.";
    // 表示未找到匹配请求参数类型的构造函数的错误信息
    public const string ShellPropertyFactoryConstructorNotFound = "No constructor found matching requested argument types.";
    // 表示对话框控件名称不能为空或 null 的错误信息
    public const string DialogControlNameCannotBeEmpty = "Dialog control name cannot be empty or null.";
    // 表示在监听线程上无法创建窗口，因为没有现有窗口的错误信息
    public const string MessageListenerNoWindowHandle = "Cannot create window on the listener thread because there is no existing window on the listener thread.";
    // 表示 GUID 未识别为已知文件夹的错误信息
    public const string FolderIdsUnknownGuid = "Guid does not identify a known folder.";
    // 表示系统上没有电池的错误信息
    public const string PowerManagerBatteryNotPresent = "Battery is not present on this system.";
    // 表示对话框中不能有重复名称的控件的错误信息
    public const string DialogCollectionCannotHaveDuplicateNames = "Dialog cannot have more than one control with the same name.";
    // 定义一个常量字符串，表示在对话框显示时无法更改启动位置
    public const string StartupLocationCannotBeChanged = "Startup location cannot be changed while dialog is showing.";
    // 定义一个常量字符串，表示未设置 TabbedThumbnailProxyWindow
    public const string TasbarWindowProxyWindowSet = "TabbedThumbnailProxyWindow has not been set.";
    // 定义一个常量字符串，表示文件类型未在应用程序中注册
    public const string JumpListFileTypeNotRegistered = "The file type is not registered with this application.";
    // 定义一个常量字符串，表示目标数组太小或数组索引无效
    public const string ShellObjectCollectionArrayTooSmall = "Destination array too small, or invalid arrayIndex.";
# 代码结束的大括号，表示一个代码块或结构体的结束
}
```