# `.\better-genshin-impact\Build\MicaSetup\Natives\Shell\Dialogs\Common\ShellObject.cs`

```cs
# 引入必要的命名空间
﻿using MicaSetup.Helper;
using System;
using System.IO;
using System.Runtime.InteropServices;
using System.Runtime.InteropServices.ComTypes;

namespace MicaSetup.Shell.Dialogs;

#pragma warning disable CS8618  # 禁用警告 CS8618，表示有可能出现未赋值的非-nullable 类型

public abstract class ShellObject : IDisposable, IEquatable<ShellObject>
{
    internal IShellItem2 nativeShellItem;  # 内部存储原生 Shell 项

    private string _internalName;  # 存储对象名称的私有字段

    private string _internalParsingName;  # 存储解析名称的私有字段

    private nint _internalPIDL = 0;  # 存储内部 PIDL（指针标识列表）的字段，默认为 0

    private int? hashValue;  # 存储哈希值的可空整数字段

    private ShellObject parentShellObject;  # 存储父 Shell 对象的字段

    private ShellThumbnail thumbnail;  # 存储缩略图的字段

    internal ShellObject()  # 内部构造函数，初始化新实例
    {
    }

    internal ShellObject(IShellItem2 shellItem) => nativeShellItem = shellItem;  # 使用传入的 IShellItem2 初始化实例

    ~ShellObject()  # 析构函数，释放资源
    {
        Dispose(false);
    }

    public static bool IsPlatformSupported => OsVersionHelper.IsWindowsVista_OrGreater;  # 判断平台是否支持，是否为 Windows Vista 或更高版本

    public bool IsFileSystemObject  # 判断对象是否为文件系统对象
    {
        get
        {
            try
            {
                NativeShellItem.GetAttributes(ShellFileGetAttributesOptions.FileSystem, out var sfgao);  # 获取文件系统属性
                return (sfgao & ShellFileGetAttributesOptions.FileSystem) != 0;  # 检查属性是否包含文件系统标志
            }
            catch (FileNotFoundException)  # 捕获文件未找到异常
            {
                return false;
            }
            catch (NullReferenceException)  # 捕获空引用异常
            {
                return false;
            }
        }
    }

    public bool IsLink  # 判断对象是否为链接
    {
        get
        {
            try
            {
                NativeShellItem.GetAttributes(ShellFileGetAttributesOptions.Link, out var sfgao);  # 获取链接属性
                return (sfgao & ShellFileGetAttributesOptions.Link) != 0;  # 检查属性是否包含链接标志
            }
            catch (FileNotFoundException)  # 捕获文件未找到异常
            {
                return false;
            }
            catch (NullReferenceException)  # 捕获空引用异常
            {
                return false;
            }
        }
    }

    public virtual string Name  # 对象名称属性
    {
        get
        {
            if (_internalName == null && NativeShellItem != null)  # 如果内部名称未设置且 NativeShellItem 不为空
            {
                var hr = NativeShellItem.GetDisplayName(ShellItemDesignNameOptions.Normal, out var pszString);  # 获取显示名称
                if (hr == HResult.Ok && pszString != 0)  # 如果成功获取显示名称
                {
                    _internalName = Marshal.PtrToStringAuto(pszString);  # 将指针转换为字符串并存储
                    Marshal.FreeCoTaskMem(pszString);  # 释放内存
                }
            }
            return _internalName!;  # 返回对象名称
        }

        protected set => _internalName = value;  # 保护访问器用于设置内部名称
    }

    public ShellObject Parent  # 父对象属性
    {
        get
        {
            # 如果父对象为空且 NativeShellItem2 不为空
            if (parentShellObject == null! && NativeShellItem2 != null)
            {
                # 获取父对象并将其存储在 parentShellItem 中
                var hr = NativeShellItem2.GetParent(out var parentShellItem);
    
                # 如果操作成功且 parentShellItem 不为空
                if (hr == HResult.Ok && parentShellItem != null)
                {
                    # 使用父项创建一个新的 ShellObject 实例
                    parentShellObject = ShellObjectFactory.Create(parentShellItem);
                }
                # 如果父项不存在，返回 null
                else if (hr == HResult.NoObject)
                {
                    return null!;
                }
                # 其他情况抛出异常
                else
                {
                    throw new ShellException(hr);
                }
            }
    
            # 返回 parentShellObject
            return parentShellObject!;
        }
    }
    
    public virtual string ParsingName
    {
        get
        {
            # 如果 _internalParsingName 为 null 且 nativeShellItem 不为空
            if (_internalParsingName == null && nativeShellItem != null)
            {
                # 获取 nativeShellItem 的解析名称并赋值给 _internalParsingName
                _internalParsingName = ShellHelper.GetParsingName(nativeShellItem);
            }
            # 返回 _internalParsingName，若为 null 则返回空字符串
            return _internalParsingName ?? string.Empty;
        }
        protected set => _internalParsingName = value;
    }
    
    public ShellThumbnail Thumbnail
    {
        get
        {
            # 如果 thumbnail 为 null，则创建一个新的 ShellThumbnail 实例
            thumbnail ??= new ShellThumbnail(this);
            # 返回 thumbnail
            return thumbnail;
        }
    }
    
    internal IPropertyStore NativePropertyStore { get; set; }
    
    internal virtual IShellItem NativeShellItem => NativeShellItem2;
    
    internal virtual IShellItem2 NativeShellItem2
    {
        get
        {
            # 如果 nativeShellItem 为空且 ParsingName 不为空
            if (nativeShellItem == null && ParsingName != null)
            {
                # 使用解析名称创建一个新的 IShellItem2 实例
                var guid = new Guid(ShellIIDGuid.IShellItem2);
                var retCode = Shell32.SHCreateItemFromParsingName(ParsingName, 0, ref guid, out nativeShellItem);
    
                # 如果 nativeShellItem 为空或操作失败，抛出异常
                if (nativeShellItem == null || !CoreErrorHelper.Succeeded(retCode))
                {
                    throw new ShellException(LocalizedMessages.ShellObjectCreationFailed, Marshal.GetExceptionForHR(retCode));
                }
            }
            # 返回 nativeShellItem
            return nativeShellItem!;
        }
    }
    
    internal virtual nint PIDL
    {
        get
        {
            # 如果 _internalPIDL 为 0 且 NativeShellItem 不为空
            if (_internalPIDL == 0 && NativeShellItem != null)
            {
                # 使用 NativeShellItem 创建 PIDL 并赋值给 _internalPIDL
                _internalPIDL = ShellHelper.PidlFromShellItem(NativeShellItem);
            }
    
            # 返回 _internalPIDL
            return _internalPIDL;
        }
        set => _internalPIDL = value;
    }
    
    public static ShellObject FromParsingName(string parsingName) => ShellObjectFactory.Create(parsingName);
    
    public static bool operator !=(ShellObject leftShellObject, ShellObject rightShellObject) => !(leftShellObject == rightShellObject);
    
    public static bool operator ==(ShellObject leftShellObject, ShellObject rightShellObject)
    {
        # 如果左侧对象为 null，则右侧对象也必须为 null
        if (leftShellObject is null)
        {
            return rightShellObject is null;
        }
        # 否则，比较两个对象是否相等
        return leftShellObject.Equals(rightShellObject);
    }
    
    public void Dispose()
    {
        # 释放资源并通知垃圾回收器
        Dispose(true);
        GC.SuppressFinalize(this);
    }
    
    public bool Equals(ShellObject other)
    {
        # 初始化一个布尔变量，默认值为 false
        var areEqual = false;

        # 检查 'other' 对象是否为 null
        if (other != null!)
        {
            # 获取当前对象的 NativeShellItem
            var ifirst = NativeShellItem;
            # 获取 'other' 对象的 NativeShellItem
            var isecond = other.NativeShellItem;
            # 检查两个 NativeShellItem 是否都不为 null
            if (ifirst != null && isecond != null)
            {
                # 比较两个 NativeShellItem，并获取比较结果
                var hr = ifirst.Compare(
                    isecond, SICHINTF.SICHINT_ALLFIELDS, out var result);

                # 如果比较成功且结果为 0，则 areEqual 设置为 true
                areEqual = (hr == HResult.Ok) && (result == 0);
            }
        }

        # 返回 areEqual 的值
        return areEqual;
    }

    # 重写 Equals 方法，比较当前对象与另一个对象是否相等
    public override bool Equals(object obj) => Equals((obj as ShellObject)!);

    # 获取显示名称的方法，根据传入的显示名称类型返回名称
    public virtual string GetDisplayName(DisplayNameType displayNameType)
    {
        # 初始化返回值为 null
        string returnValue = null!;
        # 调用 NativeShellItem2 的 GetDisplayName 方法获取名称
        NativeShellItem2?.GetDisplayName((ShellItemDesignNameOptions)displayNameType, out returnValue);
        # 返回获取到的显示名称
        return returnValue;
    }

    # 重写 GetHashCode 方法，用于计算当前对象的哈希码
    public override int GetHashCode()
    {
        # 如果 hashValue 还没有计算过
        if (!hashValue.HasValue)
        {
            # 获取 PIDL 的大小
            var size = Shell32.ILGetSize(PIDL);
            # 如果大小不为 0
            if (size != 0)
            {
                # 创建一个与 PIDL 大小相等的字节数组
                var pidlData = new byte[size];
                # 将 PIDL 数据复制到字节数组
                Marshal.Copy(PIDL, pidlData, 0, (int)size);

                # 初始化哈希计算所用的常量和初始哈希值
                const int p = 16777619;
                int hash = -2128831035;

                # 遍历字节数组中的每一个字节，更新哈希值
                for (int i = 0; i < pidlData.Length; i++)
                    hash = (hash ^ pidlData[i]) * p;

                # 将计算得到的哈希值存储到 hashValue 中
                hashValue = hash;
            }
            else
            {
                # 如果 PIDL 大小为 0，则哈希值设置为 0
                hashValue = 0;
            }
        }
        # 返回计算得到的哈希值
        return hashValue.Value;
    }

    # 重写 ToString 方法，返回对象的名称
    public override string ToString() => Name;

    # 更新对象的状态
    public void Update(IBindCtx bindContext)
    {
        # 初始化 HRESULT 值为 Ok
        var hr = HResult.Ok;

        # 如果 NativeShellItem2 不为 null，调用其 Update 方法
        if (NativeShellItem2 != null)
        {
            hr = NativeShellItem2.Update(bindContext);
        }

        # 如果更新失败，抛出 ShellException 异常
        if (CoreErrorHelper.Failed(hr))
        {
            throw new ShellException(hr);
        }
    }

    # 释放对象占用的资源
    protected virtual void Dispose(bool disposing)
    {
        # 如果 disposing 为 true，释放托管资源
        if (disposing)
        {
            _internalName = null!;
            _internalParsingName = null!;
            thumbnail = null!;
            parentShellObject = null!;
        }

        # 释放非托管资源（PIDL）
        if (_internalPIDL != 0)
        {
            Shell32.ILFree(_internalPIDL);
            _internalPIDL = 0;
        }

        # 释放 nativeShellItem 对象
        if (nativeShellItem != null)
        {
            Marshal.ReleaseComObject(nativeShellItem);
            nativeShellItem = null!;
        }

        # 释放 NativePropertyStore 对象
        if (NativePropertyStore != null)
        {
            Marshal.ReleaseComObject(NativePropertyStore);
            NativePropertyStore = null!;
        }
    }
# 结束当前代码块或代码段的标记
}
```