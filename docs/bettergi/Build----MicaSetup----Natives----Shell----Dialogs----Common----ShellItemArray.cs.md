# `.\better-genshin-impact\Build\MicaSetup\Natives\Shell\Dialogs\Common\ShellItemArray.cs`

```cs
﻿using System;
using System.Collections.Generic;

namespace MicaSetup.Shell.Dialogs;

internal class ShellItemArray : IShellItemArray
{
    // 用于存储 IShellItem 对象的列表
    private readonly List<IShellItem> shellItemsList = new();

    // 构造函数，初始化 shellItemsList，并将传入的 IShellItem 数组添加到列表中
    internal ShellItemArray(IShellItem[] shellItems) => shellItemsList.AddRange(shellItems);

    // 实现 IShellItemArray 接口方法，抛出不支持异常
    public HResult BindToHandler(nint pbc, ref Guid rbhid, ref Guid riid, out nint ppvOut) => throw new NotSupportedException();

    // 实现 IShellItemArray 接口方法，抛出不支持异常
    public HResult EnumItems(out nint ppenumShellItems) => throw new NotSupportedException();

    // 实现 IShellItemArray 接口方法，抛出不支持异常
    public HResult GetAttributes(ShellItemAttributeOptions dwAttribFlags, ShellFileGetAttributesOptions sfgaoMask, out ShellFileGetAttributesOptions psfgaoAttribs) => throw new NotSupportedException();

    // 获取列表中项目的数量
    public HResult GetCount(out uint pdwNumItems)
    {
        // 设置 pdwNumItems 为 shellItemsList 的元素数量
        pdwNumItems = (uint)shellItemsList.Count;
        // 返回成功状态
        return HResult.Ok;
    }

    // 根据索引获取列表中的项
    public HResult GetItemAt(uint dwIndex, out IShellItem ppsi)
    {
        var index = (int)dwIndex;

        if (index < shellItemsList.Count)
        {
            // 如果索引有效，返回对应的 IShellItem
            ppsi = shellItemsList[index];
            return HResult.Ok;
        }
        else
        {
            // 如果索引无效，返回 null，并且失败状态
            ppsi = null!;
            return HResult.Fail;
        }
    }

    // 实现 IShellItemArray 接口方法，抛出不支持异常
    public HResult GetPropertyDescriptionList(ref PropertyKey keyType, ref Guid riid, out nint ppv) => throw new NotSupportedException();

    // 实现 IShellItemArray 接口方法，抛出不支持异常
    public HResult GetPropertyStore(int Flags, ref Guid riid, out nint ppv) => throw new NotSupportedException();
}
```