# `.\better-genshin-impact\Build\MicaSetup\Natives\Shell\Dialogs\CommonFileDialogs\CommonOpenFileDialog.cs`

```cs
# 引入需要的系统库和命名空间
using System.Collections.Generic;
using System.Collections.ObjectModel;
using System.Diagnostics;
using System.Runtime.InteropServices;

namespace MicaSetup.Shell.Dialogs;

#pragma warning disable CS8618  # 禁用 CS8618 警告（未初始化的非空字段）

public sealed class CommonOpenFileDialog : CommonFileDialog
{
    private bool allowNonFileSystem;  # 是否允许非文件系统项目
    private bool isFolderPicker;  # 是否为文件夹选择器
    private bool multiselect;  # 是否允许多选
    private NativeFileOpenDialog openDialogCoClass;  # Native 文件打开对话框的 COM 类

    public CommonOpenFileDialog() : base() => EnsureReadOnly = true;  # 默认构造函数，设置只读属性

    public CommonOpenFileDialog(string name) : base(name) => EnsureReadOnly = true;  # 带名称的构造函数，设置只读属性

    public bool AllowNonFileSystemItems
    {
        get => allowNonFileSystem;  # 获取是否允许非文件系统项目
        set => allowNonFileSystem = value;  # 设置是否允许非文件系统项目
    }

    public IEnumerable<string> FileNames
    {
        get
        {
            CheckFileNamesAvailable();  # 检查文件名是否可用
            return base.FileNameCollection;  # 返回文件名集合
        }
    }

    public ICollection<ShellObject> FilesAsShellObject
    {
        get
        {
            CheckFileItemsAvailable();  # 检查文件项目是否可用

            ICollection<ShellObject> resultItems = new Collection<ShellObject>();  # 创建结果集合

            foreach (var si in items)  # 遍历项目
            {
                resultItems.Add(ShellObjectFactory.Create(si));  # 将项目转换为 ShellObject 并添加到集合
            }

            return resultItems;  # 返回结果集合
        }
    }

    public bool IsFolderPicker
    {
        get => isFolderPicker;  # 获取是否为文件夹选择器
        set => isFolderPicker = value;  # 设置是否为文件夹选择器
    }

    public bool Multiselect
    {
        get => multiselect;  # 获取是否允许多选
        set => multiselect = value;  # 设置是否允许多选
    }

    internal override void CleanUpNativeFileDialog()
    {
        if (openDialogCoClass != null)  # 如果 COM 对象存在
        {
            Marshal.ReleaseComObject(openDialogCoClass);  # 释放 COM 对象
        }
    }

    internal override FileOpenOptions GetDerivedOptionFlags(FileOpenOptions flags)
    {
        if (multiselect)  # 如果允许多选
        {
            flags |= FileOpenOptions.AllowMultiSelect;  # 添加多选标志
        }
        if (isFolderPicker)  # 如果是文件夹选择器
        {
            flags |= FileOpenOptions.PickFolders;  # 添加选择文件夹标志
        }

        if (!allowNonFileSystem)  # 如果不允许非文件系统项目
        {
            flags |= FileOpenOptions.ForceFilesystem;  # 强制文件系统标志
        }
        else if (allowNonFileSystem)  # 如果允许非文件系统项目
        {
            flags |= FileOpenOptions.AllNonStorageItems;  # 允许所有非存储项目标志
        }

        return flags;  # 返回更新后的标志
    }

    internal override IFileDialog GetNativeFileDialog()
    {
        Debug.Assert(openDialogCoClass != null, "Must call Initialize() before fetching dialog interface");  # 断言 COM 对象已初始化

        return openDialogCoClass!;  # 返回 COM 对象
    }

    internal override void InitializeNativeFileDialog()
    {
        openDialogCoClass ??= new NativeFileOpenDialog();  # 如果 COM 对象未初始化，则进行初始化
    }

    internal override void PopulateWithFileNames(Collection<string> names)
    {
        openDialogCoClass.GetResults(out var resultsArray);  # 从 COM 对象获取结果数组
        resultsArray.GetCount(out var count);  # 获取结果数量
        names.Clear();  # 清空名称集合
        for (var i = 0; i < count; i++)  # 遍历每个结果
        {
            names.Add(GetFileNameFromShellItem(GetShellItemAt(resultsArray, i)));  # 获取文件名并添加到集合
        }
    }

    internal override void PopulateWithIShellItems(Collection<IShellItem> items)
    # 从 openDialogCoClass 对象获取结果数组
    openDialogCoClass.GetResults(out var resultsArray);
    # 从结果数组中获取项目数量
    resultsArray.GetCount(out var count);
    # 清空当前的项目集合
    items.Clear();
    # 遍历每一个项目，根据索引获取并添加到项目集合中
    for (var i = 0; i < count; i++)
    {
        # 从结果数组中获取指定索引的项目，并将其添加到项目集合中
        items.Add(GetShellItemAt(resultsArray, i));
    }
# 结束当前代码块或函数的定义
}
```