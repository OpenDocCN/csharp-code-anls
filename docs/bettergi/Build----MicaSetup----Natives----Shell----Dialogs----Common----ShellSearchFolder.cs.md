# `.\better-genshin-impact\Build\MicaSetup\Natives\Shell\Dialogs\Common\ShellSearchFolder.cs`

```cs
# 引用 MicaSetup.Helper 命名空间和其他必要的命名空间
﻿using MicaSetup.Helper;
using System;
using System.Collections.Generic;
using System.Linq;

# 定义命名空间 MicaSetup.Shell.Dialogs
namespace MicaSetup.Shell.Dialogs;

# 关闭特定的编译器警告
#pragma warning disable CS8618

# 定义 ShellSearchFolder 类，继承自 ShellSearchCollection
public class ShellSearchFolder : ShellSearchCollection
{
    # 声明私有字段 searchCondition
    private SearchCondition searchCondition;

    # 声明私有字段 searchScopePaths
    private string[] searchScopePaths;

    # 定义构造函数，接受 SearchCondition 和 ShellContainer 数组作为参数
    public ShellSearchFolder(SearchCondition searchCondition, params ShellContainer[] searchScopePath)
    {
        # 调用辅助方法，确保操作系统版本为 Vista 或更高版本
        OsVersionHelper.ThrowIfNotVista();

        # 初始化 NativeSearchFolderItemFactory 为 SearchFolderItemFactoryCoClass 的实例
        NativeSearchFolderItemFactory = (ISearchFolderItemFactory)new SearchFolderItemFactoryCoClass();

        # 设置 SearchCondition 属性
        SearchCondition = searchCondition;

        # 如果 searchScopePath 不为空且包含元素，则设置 SearchScopePaths 属性
        if (searchScopePath != null && searchScopePath.Length > 0 && searchScopePath[0] != null!)
        {
            SearchScopePaths = searchScopePath.Select(cont => cont.ParsingName);
        }
    }

    # 定义另一个构造函数，接受 SearchCondition 和字符串数组作为参数
    public ShellSearchFolder(SearchCondition searchCondition, params string[] searchScopePath)
    {
        # 调用辅助方法，确保操作系统版本为 Vista 或更高版本
        OsVersionHelper.ThrowIfNotVista();

        # 初始化 NativeSearchFolderItemFactory 为 SearchFolderItemFactoryCoClass 的实例
        NativeSearchFolderItemFactory = (ISearchFolderItemFactory)new SearchFolderItemFactoryCoClass();

        # 如果 searchScopePath 不为空且包含元素，则设置 SearchScopePaths 属性
        if (searchScopePath != null && searchScopePath.Length > 0 && searchScopePath[0] != null)
        {
            SearchScopePaths = searchScopePath;
        }

        # 设置 SearchCondition 属性
        SearchCondition = searchCondition;
    }

    # 定义 SearchCondition 属性，具有 getter 和 private setter
    public SearchCondition SearchCondition
    {
        get => searchCondition;
        private set
        {
            # 设置 searchCondition 字段的值
            searchCondition = value;

            # 调用 NativeSearchFolderItemFactory 的 SetCondition 方法设置搜索条件
            NativeSearchFolderItemFactory.SetCondition(searchCondition.NativeSearchCondition);
        }
    }

    # 定义 SearchScopePaths 属性，具有 getter 和 private setter
    public IEnumerable<string> SearchScopePaths
    {
        get
        {
            # 遍历 searchScopePaths 数组并逐一返回元素
            foreach (var scopePath in searchScopePaths)
            {
                yield return scopePath;
            }
        }
        private set
        {
            # 将传入的值转换为数组并赋给 searchScopePaths
            searchScopePaths = value.ToArray();
            var shellItems = new List<IShellItem>(searchScopePaths.Length);

            # 创建一个 GUID 实例，用于标识 IShellItem 接口
            var shellItemGuid = new Guid(ShellIIDGuid.IShellItem);

            # 遍历 searchScopePaths 中的每个路径，创建对应的 IShellItem 实例
            foreach (var path in searchScopePaths)
            {
                var hr = Shell32.SHCreateItemFromParsingName(path, 0, ref shellItemGuid, out IShellItem scopeShellItem);

                # 如果成功，则将 IShellItem 实例添加到 shellItems 列表中
                if (CoreErrorHelper.Succeeded(hr)) { shellItems.Add(scopeShellItem); }
            }

            # 创建 IShellItemArray 实例并设置为 NativeSearchFolderItemFactory 的作用域
            IShellItemArray scopeShellItemArray = new ShellItemArray(shellItems.ToArray());

            # 调用 NativeSearchFolderItemFactory 的 SetScope 方法设置搜索范围
            var hResult = NativeSearchFolderItemFactory.SetScope(scopeShellItemArray);

            # 如果调用失败，则抛出 ShellException 异常
            if (!CoreErrorHelper.Succeeded((int)hResult)) { throw new ShellException((int)hResult); }
        }
    }

    # 定义内部属性 NativeSearchFolderItemFactory
    internal ISearchFolderItemFactory NativeSearchFolderItemFactory { get; set; }

    # 重写 NativeShellItem 属性
    internal override IShellItem NativeShellItem
    {
        get
        {
            // 创建一个新的 Guid 对象，用于标识 IShellItem
            var guid = new Guid(ShellIIDGuid.IShellItem);

            // 检查 NativeSearchFolderItemFactory 是否为 null
            // 如果为 null，则返回 null
            if (NativeSearchFolderItemFactory == null) { return null!; }

            // 调用 NativeSearchFolderItemFactory 的 GetShellItem 方法
            // 传入 guid 作为参数，并通过 out 变量 shellItem 获取结果
            var hr = NativeSearchFolderItemFactory.GetShellItem(ref guid, out var shellItem);

            // 检查方法调用是否成功，如果失败则抛出 ShellException 异常
            if (!CoreErrorHelper.Succeeded(hr)) { throw new ShellException(hr); }

            // 返回获取到的 shellItem 对象
            return shellItem;
        }
    }
# 结束当前的代码块或结构体（在大多数编程语言中，这表示代码块的结束）
}
```