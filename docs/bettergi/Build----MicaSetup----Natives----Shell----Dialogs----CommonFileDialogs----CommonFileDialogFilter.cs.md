# `.\better-genshin-impact\Build\MicaSetup\Natives\Shell\Dialogs\CommonFileDialogs\CommonFileDialogFilter.cs`

```cs
using System;
using System.Collections.ObjectModel;
using System.Text;

namespace MicaSetup.Shell.Dialogs;

#pragma warning disable CS8618
# 禁用 CS8618 警告，这个警告表示非空字段未初始化

public class CommonFileDialogFilter
{
    # 定义一个只读的字符串集合来存储文件扩展名
    private readonly Collection<string> extensions;
    # 定义一个字符串来存储原始显示名称
    private string rawDisplayName;

    # 标志是否显示扩展名
    private bool showExtensions = true;

    # 默认构造函数
    public CommonFileDialogFilter() => extensions = new Collection<string>();

    # 带有原始显示名称和扩展名列表的构造函数
    public CommonFileDialogFilter(string rawDisplayName, string extensionList) : this()
    {
        # 如果扩展名列表为空或为 null，抛出 ArgumentNullException 异常
        if (string.IsNullOrEmpty(extensionList))
        {
            throw new ArgumentNullException("extensionList");
        }

        # 设置原始显示名称
        this.rawDisplayName = rawDisplayName;

        # 将扩展名列表按逗号或分号分隔，转换为扩展名数组
        var rawExtensions = extensionList.Split(',', ';');
        # 遍历扩展名数组并将其标准化后添加到集合中
        foreach (var extension in rawExtensions)
        {
            extensions.Add(CommonFileDialogFilter.NormalizeExtension(extension));
        }
    }

    # 显示名称属性，既可以获取也可以设置显示名称
    public string DisplayName
    {
        get
        {
            # 如果需要显示扩展名，则格式化显示名称和扩展名列表
            if (showExtensions)
            {
                return string.Format(System.Globalization.CultureInfo.InvariantCulture,
                    "{0} ({1})",
                    rawDisplayName,
                    CommonFileDialogFilter.GetDisplayExtensionList(extensions));
            }

            # 否则只返回原始显示名称
            return rawDisplayName;
        }

        set
        {
            # 如果设置的显示名称为空或为 null，抛出 ArgumentNullException 异常
            if (string.IsNullOrEmpty(value))
            {
                throw new ArgumentNullException("value");
            }
            # 设置原始显示名称
            rawDisplayName = value;
        }
    }

    # 扩展名集合属性，只读
    public Collection<string> Extensions => extensions;

    # 是否显示扩展名的属性，可读可写
    public bool ShowExtensions
    {
        get => showExtensions;
        set => showExtensions = value;
    }

    # 重写 ToString 方法，格式化输出显示名称和扩展名列表
    public override string ToString() => string.Format(System.Globalization.CultureInfo.InvariantCulture,
            "{0} ({1})",
            rawDisplayName,
            CommonFileDialogFilter.GetDisplayExtensionList(extensions));

    # 获取过滤器规范（FilterSpec），用于表示文件对话框的过滤条件
    internal FilterSpec GetFilterSpec()
    {
        # 使用 StringBuilder 构建过滤器字符串
        var filterList = new StringBuilder();
        # 遍历扩展名集合，构建过滤器字符串
        foreach (var extension in extensions)
        {
            if (filterList.Length > 0) { filterList.Append(";"); }

            filterList.Append("*.");
            filterList.Append(extension);
        }
        # 返回新的 FilterSpec 对象
        return new FilterSpec(DisplayName, filterList.ToString());
    }

    # 静态方法，获取扩展名列表的显示字符串
    private static string GetDisplayExtensionList(Collection<string> extensions)
    {
        # 使用 StringBuilder 构建扩展名显示列表
        var extensionList = new StringBuilder();
        # 遍历扩展名集合，构建扩展名显示字符串
        foreach (var extension in extensions)
        {
            if (extensionList.Length > 0) { extensionList.Append(", "); }
            extensionList.Append("*.");
            extensionList.Append(extension);
        }

        # 返回扩展名显示字符串
        return extensionList.ToString();
    }

    # 静态方法，标准化扩展名（方法体未完成）
    private static string NormalizeExtension(string rawExtension)
    # 去除原始扩展名字符串两端的空白字符
    rawExtension = rawExtension.Trim();
    # 替换扩展名字符串中的 "*." 为 null
    rawExtension = rawExtension.Replace("*.", null);

    # 查找扩展名字符串中第一个 '.' 的位置
    int indexOfDot = rawExtension.IndexOf('.');
    # 如果找到 '.'，则执行以下操作
    if (indexOfDot != -1)
    {
        # 从扩展名字符串中移除 '.' 及其后的内容
        rawExtension.Remove(indexOfDot);
    }

    # 返回处理后的扩展名字符串
    return rawExtension;
请提供需要注释的代码段。
```