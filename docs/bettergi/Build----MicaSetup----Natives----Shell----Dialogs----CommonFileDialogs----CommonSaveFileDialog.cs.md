# `.\better-genshin-impact\Build\MicaSetup\Natives\Shell\Dialogs\CommonFileDialogs\CommonSaveFileDialog.cs`

```cs
# 使用系统命名空间和库
using System;
using System.Diagnostics;
using System.Runtime.InteropServices;
using System.Text;

namespace MicaSetup.Shell.Dialogs;

# 禁用特定的编译器警告
#pragma warning disable CS8073
#pragma warning disable CS8618

# 定义 CommonSaveFileDialog 类，继承自 CommonFileDialog
public sealed class CommonSaveFileDialog : CommonFileDialog
{
    # 定义类的私有字段
    private bool alwaysAppendDefaultExtension;
    private bool createPrompt;
    private bool isExpandedMode;
    private bool overwritePrompt = true;
    private NativeFileSaveDialog saveDialogCoClass;

    # 默认构造函数
    public CommonSaveFileDialog()
    {
    }

    # 带名称的构造函数，调用基类构造函数
    public CommonSaveFileDialog(string name) : base(name)
    {
    }

    # 属性：始终附加默认扩展名
    public bool AlwaysAppendDefaultExtension
    {
        get => alwaysAppendDefaultExtension;
        set
        {
            # 如果对话框正在显示，抛出异常
            ThrowIfDialogShowing(LocalizedMessages.AlwaysAppendDefaultExtensionCannotBeChanged);
            alwaysAppendDefaultExtension = value;
        }
    }

    # 属性：收集的属性
    public ShellPropertyCollection CollectedProperties
    {
        get
        {
            # 初始化本地文件对话框
            InitializeNativeFileDialog();
            # 获取本地文件对话框接口
            var nativeDialog = GetNativeFileDialog() as IFileSaveDialog;

            if (nativeDialog != null)
            {
                # 获取对话框属性
                var hr = nativeDialog.GetProperties(out var propertyStore);

                if (propertyStore != null && CoreErrorHelper.Succeeded(hr))
                {
                    # 返回包含属性的 ShellPropertyCollection
                    return new ShellPropertyCollection(propertyStore);
                }
            }

            # 返回 null，如果没有获取到属性
            return null!;
        }
    }

    # 属性：创建提示
    public bool CreatePrompt
    {
        get => createPrompt;
        set
        {
            # 如果对话框正在显示，抛出异常
            ThrowIfDialogShowing(LocalizedMessages.CreatePromptCannotBeChanged);
            createPrompt = value;
        }
    }

    # 属性：扩展模式
    public bool IsExpandedMode
    {
        get => isExpandedMode;
        set
        {
            # 如果对话框正在显示，抛出异常
            ThrowIfDialogShowing(LocalizedMessages.IsExpandedModeCannotBeChanged);
            isExpandedMode = value;
        }
    }

    # 属性：覆盖提示
    public bool OverwritePrompt
    {
        get => overwritePrompt;
        set
        {
            # 如果对话框正在显示，抛出异常
            ThrowIfDialogShowing(LocalizedMessages.OverwritePromptCannotBeChanged);
            overwritePrompt = value;
        }
    }

    # 方法：设置收集的属性键
    public void SetCollectedPropertyKeys(bool appendDefault, params PropertyKey[] propertyList)
    {
        # 检查 propertyList 是否不为空且第一个元素不为空
        if (propertyList != null && propertyList.Length > 0 && propertyList[0] != null)
        {
            # 创建一个 StringBuilder 实例，用于构建属性描述字符串
            var sb = new StringBuilder("prop:");
            # 遍历 propertyList 中的每个键
            foreach (var key in propertyList)
            {
                # 从缓存中获取属性描述的规范名称
                var canonicalName = ShellPropertyDescriptionsCache.Cache.GetPropertyDescription(key).CanonicalName;
                # 如果规范名称不为空，则将其添加到 StringBuilder 中
                if (!string.IsNullOrEmpty(canonicalName)) { sb.AppendFormat("{0};", canonicalName); }
            }

            # 创建一个 GUID 对象，用于指定属性描述列表的类型
            var guid = new Guid(ShellIIDGuid.IPropertyDescriptionList);
            # 初始化属性描述列表对象
            IPropertyDescriptionList propertyDescriptionList = null!;

            try
            {
                # 从属性描述字符串获取属性描述列表
                var hr = PropertySystemNativeMethods.PSGetPropertyDescriptionListFromString(
                    sb.ToString(),
                    ref guid,
                    out propertyDescriptionList);

                # 如果成功获取属性描述列表
                if (CoreErrorHelper.Succeeded(hr))
                {
                    # 初始化原生文件对话框
                    InitializeNativeFileDialog();
                    # 获取原生文件对话框对象
                    var nativeDialog = GetNativeFileDialog() as IFileSaveDialog;

                    # 如果成功获取文件保存对话框
                    if (nativeDialog != null)
                    {
                        # 设置收集的属性
                        hr = nativeDialog.SetCollectedProperties(propertyDescriptionList, appendDefault);

                        # 如果设置属性失败，则抛出异常
                        if (!CoreErrorHelper.Succeeded(hr))
                        {
                            throw new ShellException(hr);
                        }
                    }
                }
            }
            finally
            {
                # 确保释放属性描述列表对象
                if (propertyDescriptionList != null)
                {
                    Marshal.ReleaseComObject(propertyDescriptionList);
                }
            }
        }
    }

    # 设置保存为项目的方法
    public void SetSaveAsItem(ShellObject item)
    {
        # 如果项目为空，抛出参数为空异常
        if (item == null!)
        {
            throw new ArgumentNullException("item");
        }

        # 初始化原生文件对话框
        InitializeNativeFileDialog();
        # 获取原生文件对话框对象
        var nativeDialog = GetNativeFileDialog() as IFileSaveDialog;

        # 如果成功获取文件保存对话框
        if (nativeDialog != null)
        {
            # 设置要保存的项目
            nativeDialog.SetSaveAsItem(item.NativeShellItem);
        }
    }

    # 清理原生文件对话框的方法
    internal override void CleanUpNativeFileDialog()
    {
        # 如果保存对话框 COM 对象不为空，则释放它
        if (saveDialogCoClass != null)
        {
            Marshal.ReleaseComObject(saveDialogCoClass);
        }
    }

    # 获取派生的文件打开选项标志
    internal override FileOpenOptions GetDerivedOptionFlags(FileOpenOptions flags)
    {
        # 如果需要提示覆盖，则设置标志
        if (overwritePrompt)
        {
            flags |= FileOpenOptions.OverwritePrompt;
        }
        # 如果需要提示创建，则设置标志
        if (createPrompt)
        {
            flags |= FileOpenOptions.CreatePrompt;
        }
        # 如果不是扩展模式，则设置标志
        if (!isExpandedMode)
        {
            flags |= FileOpenOptions.DefaultNoMiniMode;
        }
        # 如果总是附加默认扩展名，则设置标志
        if (alwaysAppendDefaultExtension)
        {
            flags |= FileOpenOptions.StrictFileTypes;
        }
        # 返回更新后的标志
        return flags;
    }

    # 获取原生文件对话框的方法
    internal override IFileDialog GetNativeFileDialog()
    {
        # 确保在获取对话框接口之前调用了初始化
        Debug.Assert(saveDialogCoClass != null, "Must call Initialize() before fetching dialog interface");
        # 返回保存对话框 COM 对象
        return saveDialogCoClass!;
    }
    internal override void InitializeNativeFileDialog()
    {
        # 如果 saveDialogCoClass 为空，则创建一个新的 NativeFileSaveDialog 实例
        if (saveDialogCoClass == null)
        {
            saveDialogCoClass = new NativeFileSaveDialog();
        }
    }

    internal override void PopulateWithFileNames(
        System.Collections.ObjectModel.Collection<string> names)
    {
        # 从 saveDialogCoClass 获取结果项
        saveDialogCoClass.GetResult(out var item);

        # 如果结果项为空，则抛出异常
        if (item == null)
        {
            throw new InvalidOperationException(LocalizedMessages.SaveFileNullItem);
        }
        # 清空 names 集合，并将获取的文件名添加到集合中
        names.Clear();
        names.Add(GetFileNameFromShellItem(item));
    }

    internal override void PopulateWithIShellItems(System.Collections.ObjectModel.Collection<IShellItem> items)
    {
        # 从 saveDialogCoClass 获取结果项
        saveDialogCoClass.GetResult(out var item);

        # 如果结果项为空，则抛出异常
        if (item == null)
        {
            throw new InvalidOperationException(LocalizedMessages.SaveFileNullItem);
        }
        # 清空 items 集合，并将获取的 IShellItem 添加到集合中
        items.Clear();
        items.Add(item);
    }
请提供您需要注释的代码段。
```