# `.\better-genshin-impact\Build\MicaSetup\Natives\Shell\Dialogs\Common\ShellFolderItems.cs`

```cs
# 引入必要的命名空间
﻿using System.Collections;
using System.Collections.Generic;
using System.Runtime.InteropServices;

namespace MicaSetup.Shell.Dialogs;

# 禁用警告 CS8618（未初始化的非空字段）
#pragma warning disable CS8618

# 内部类，用于枚举 Shell 文件夹中的项目，实现 IEnumerator<ShellObject> 接口
internal class ShellFolderItems : IEnumerator<ShellObject>
{
    # 原生 Shell 文件夹对象
    private readonly ShellContainer nativeShellFolder;
    # 当前项
    private ShellObject currentItem;
    # 原生枚举 ID 列表接口
    private IEnumIDList nativeEnumIdList;

    # 构造函数，初始化 ShellFolderItems 实例
    internal ShellFolderItems(ShellContainer nativeShellFolder)
    {
        # 保存传入的原生 Shell 文件夹对象
        this.nativeShellFolder = nativeShellFolder;

        # 调用原生 Shell 文件夹的 EnumObjects 方法来获取枚举 ID 列表
        var hr = nativeShellFolder.NativeShellFolder.EnumObjects(
            0,
            ShellFolderEnumerationOptions.Folders | ShellFolderEnumerationOptions.NonFolders,
            out nativeEnumIdList);

        # 检查是否成功获取枚举 ID 列表
        if (!CoreErrorHelper.Succeeded(hr))
        {
            # 如果失败，检查错误码并抛出相应异常
            if (hr == HResult.Canceled)
            {
                throw new System.IO.FileNotFoundException();
            }
            else
            {
                throw new ShellException(hr);
            }
        }
    }

    # 实现 IEnumerator<ShellObject> 接口的 Current 属性
    public ShellObject Current => currentItem;

    # 实现 IEnumerator 接口的 Current 属性（用于非泛型枚举）
    object IEnumerator.Current => currentItem;

    # 实现 IDisposable 接口的 Dispose 方法
    public void Dispose()
    {
        # 释放原生枚举 ID 列表接口资源
        if (nativeEnumIdList != null)
        {
            Marshal.ReleaseComObject(nativeEnumIdList);
            nativeEnumIdList = null!;
        }
    }

    # 实现 IEnumerator<ShellObject> 接口的 MoveNext 方法
    public bool MoveNext()
    {
        # 检查枚举 ID 列表是否为 null
        if (nativeEnumIdList == null) { return false; }

        # 请求一个项目
        uint itemsRequested = 1;
        var hr = nativeEnumIdList.Next(itemsRequested, out var item, out var numItemsReturned);

        # 检查是否成功获取到项目
        if (numItemsReturned < itemsRequested || hr != HResult.Ok) { return false; }

        # 使用工厂方法创建 ShellObject 实例
        currentItem = ShellObjectFactory.Create(item, nativeShellFolder);

        # 成功获取到项目，返回 true
        return true;
    }

    # 实现 IEnumerator 接口的 Reset 方法
    public void Reset()
    {
        # 重置枚举 ID 列表的迭代状态
        if (nativeEnumIdList != null)
        {
            nativeEnumIdList.Reset();
        }
    }
}
```