# `.\better-genshin-impact\Build\MicaSetup\Natives\Shell\Dialogs\Common\ShellObjectCollection.cs`

```cs
﻿using System;
using System.Collections;
using System.Collections.Generic;
using System.IO;
using System.Runtime.InteropServices;

namespace MicaSetup.Shell.Dialogs;

public class ShellObjectCollection : IEnumerable, IDisposable, IList<ShellObject>
{
    // 存储 ShellObject 对象的列表
    private readonly List<ShellObject> content = new();
    // 标识集合是否只读
    private readonly bool readOnly;
    // 标识是否已被释放
    private bool isDisposed;

    // 默认构造函数
    public ShellObjectCollection()
    {
    }

    // 根据 IShellItemArray 和只读标志初始化集合
    internal ShellObjectCollection(IShellItemArray iArray, bool readOnly)
    {
        this.readOnly = readOnly;

        // 如果 iArray 不为空
        if (iArray != null)
        {
            try
            {
                // 获取 iArray 中的项目数量
                iArray.GetCount(out var itemCount);
                // 设置 content 列表的容量为项目数量
                content.Capacity = (int)itemCount;
                // 遍历每个项目并将其添加到集合中
                for (uint index = 0; index < itemCount; index++)
                {
                    iArray.GetItemAt(index, out var iShellItem);
                    content.Add(ShellObjectFactory.Create(iShellItem));
                }
            }
            finally
            {
                // 释放 iArray 相关的 COM 对象
                Marshal.ReleaseComObject(iArray);
            }
        }
    }

    // 析构函数，释放资源
    ~ShellObjectCollection()
    {
        Dispose(false);
    }

    // 判断集合是否只读
    public bool IsReadOnly => readOnly;

    // 返回集合中的项数
    public int Count => content.Count;

    // ICollection 接口实现，返回项数
    int ICollection<ShellObject>.Count => content.Count;

    // 通过索引访问或设置项
    public ShellObject this[int index]
    {
        get => content[index];
        set
        {
            // 如果集合是只读，抛出异常
            if (readOnly)
            {
                throw new InvalidOperationException(LocalizedMessages.ShellObjectCollectionInsertReadOnly);
            }

            // 设置指定索引处的项
            content[index] = value;
        }
    }

    // 从 IDataObject 创建 ShellObjectCollection 实例
    public static ShellObjectCollection FromDataObject(System.Runtime.InteropServices.ComTypes.IDataObject dataObject)
    {
        var iid = new Guid(ShellIIDGuid.IShellItemArray);
        // 从 IDataObject 创建 ShellItemArray
        Shell32.SHCreateShellItemArrayFromDataObject(dataObject, ref iid, out var shellItemArray);
        // 返回一个新的 ShellObjectCollection 实例
        return new ShellObjectCollection(shellItemArray, true);
    }

    // 向集合中添加项
    public void Add(ShellObject item)
    {
        // 如果集合是只读，抛出异常
        if (readOnly)
        {
            throw new InvalidOperationException(LocalizedMessages.ShellObjectCollectionInsertReadOnly);
        }

        // 添加项到集合中
        content.Add(item);
    }

    public MemoryStream BuildShellIDList()
    {
        # 如果内容集合为空，则抛出无效操作异常
        if (content.Count == 0)
        {
            throw new InvalidOperationException(LocalizedMessages.ShellObjectCollectionEmptyCollection);
        }

        # 创建内存流对象
        var mstream = new MemoryStream();
        try
        {
            # 创建二进制写入器以写入内存流
            var bwriter = new BinaryWriter(mstream);

            # 计算项的数量（内容数量 + 1）
            var itemCount = (uint)(content.Count + 1);

            # 创建 PIDL 数组
            var idls = new nint[itemCount];

            # 填充 PIDL 数组
            for (var index = 0; index < itemCount; index++)
            {
                if (index == 0)
                {
                    # 第一个元素是桌面文件夹的 PIDL
                    idls[index] = ((ShellObject)KnownFolders.Desktop).PIDL;
                }
                else
                {
                    # 其他元素是内容集合中的 PIDL
                    idls[index] = content[index - 1].PIDL;
                }
            }

            # 创建偏移量数组
            var offsets = new uint[itemCount + 1];
            for (var index = 0; index < itemCount; index++)
            {
                if (index == 0)
                {
                    # 第一个偏移量
                    offsets[0] = (uint)(sizeof(uint) * (offsets.Length + 1));
                }
                else
                {
                    # 后续偏移量基于之前的偏移量和 PIDL 大小
                    offsets[index] = offsets[index - 1] + Shell32.ILGetSize(idls[index - 1]);
                }
            }

            # 写入内容的计数
            bwriter.Write(content.Count);
            # 写入所有偏移量
            foreach (var offset in offsets)
            {
                bwriter.Write(offset);
            }

            # 写入所有 PIDL 数据
            foreach (var idl in idls)
            {
                var data = new byte[Shell32.ILGetSize(idl)];
                Marshal.Copy(idl, data, 0, data.Length);
                bwriter.Write(data, 0, data.Length);
            }
        }
        catch
        {
            # 异常时释放内存流资源
            mstream.Dispose();
            throw;
        }
        # 返回内存流对象
        return mstream;
    }

    # 清空内容集合，如果是只读，则抛出无效操作异常
    public void Clear()
    {
        if (readOnly)
        {
            throw new InvalidOperationException(LocalizedMessages.ShellObjectCollectionRemoveReadOnly);
        }

        content.Clear();
    }

    # 判断集合是否包含指定项
    public bool Contains(ShellObject item) => content.Contains(item);

    # 复制内容到指定数组
    public void CopyTo(ShellObject[] array, int arrayIndex)
    {
        if (array == null) { throw new ArgumentNullException("array"); }
        if (array.Length < arrayIndex + content.Count)
        {
            throw new ArgumentException(LocalizedMessages.ShellObjectCollectionArrayTooSmall, "array");
        }

        # 将内容复制到指定数组的相应位置
        for (var index = 0; index < content.Count; index++)
        {
            array[index + arrayIndex] = content[index];
        }
    }

    # 释放资源并抑制终结器
    public void Dispose()
    {
        Dispose(true);
        GC.SuppressFinalize(this);
    }

    # 获取枚举器，遍历内容集合
    public System.Collections.IEnumerator GetEnumerator()
    {
        foreach (var obj in content)
        {
            yield return obj;
        }
    }

    # 获取指定项的索引
    public int IndexOf(ShellObject item) => content.IndexOf(item);

    # 在指定位置插入新项，如果是只读，则抛出无效操作异常
    public void Insert(int index, ShellObject item)
    {
        if (readOnly)
        {
            throw new InvalidOperationException(LocalizedMessages.ShellObjectCollectionInsertReadOnly);
        }

        content.Insert(index, item);
    }
    public bool Remove(ShellObject item)
    {
        // 检查集合是否为只读
        if (readOnly)
        {
            // 如果是只读，则抛出异常
            throw new InvalidOperationException(LocalizedMessages.ShellObjectCollectionRemoveReadOnly);
        }

        // 从集合中移除指定的 ShellObject，并返回操作是否成功
        return content.Remove(item);
    }

    public void RemoveAt(int index)
    {
        // 检查集合是否为只读
        if (readOnly)
        {
            // 如果是只读，则抛出异常
            throw new InvalidOperationException(LocalizedMessages.ShellObjectCollectionRemoveReadOnly);
        }

        // 根据索引从集合中移除元素
        content.RemoveAt(index);
    }

    IEnumerator<ShellObject> IEnumerable<ShellObject>.GetEnumerator()
    {
        // 遍历集合中的每个 ShellObject
        foreach (var obj in content)
        {
            // 返回集合中的当前元素
            yield return obj;
        }
    }

    protected virtual void Dispose(bool disposing)
    {
        // 检查对象是否已经被处置
        if (isDisposed == false)
        {
            // 如果处置正在进行
            if (disposing)
            {
                // 遍历并处置集合中的每个 ShellObject
                foreach (var shellObject in content)
                {
                    shellObject.Dispose();
                }

                // 清空集合
                content.Clear();
            }

            // 标记对象为已处置
            isDisposed = true;
        }
    }
# 结束代码块
}
```