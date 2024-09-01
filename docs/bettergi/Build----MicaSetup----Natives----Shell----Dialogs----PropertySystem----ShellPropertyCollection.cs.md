# `.\better-genshin-impact\Build\MicaSetup\Natives\Shell\Dialogs\PropertySystem\ShellPropertyCollection.cs`

```cs
# 引入所需的命名空间
﻿using System;
using System.Collections.Generic;
using System.Collections.ObjectModel;
using System.Runtime.InteropServices;

namespace MicaSetup.Shell.Dialogs;

# 禁用特定的编译器警告
#pragma warning disable CS8618

# 定义一个只读集合类，继承自 ReadOnlyCollection，且实现 IDisposable 接口
public class ShellPropertyCollection : ReadOnlyCollection<IShellProperty>, IDisposable
{
    # 构造函数：根据 ShellObject 对象初始化集合
    public ShellPropertyCollection(ShellObject parent)
        : base(new List<IShellProperty>())
    {
        # 设置父级 ShellObject 对象
        ParentShellObject = parent;
        IPropertyStore nativePropertyStore = null!;
        try
        {
            # 创建默认的 IPropertyStore 实例
            nativePropertyStore = CreateDefaultPropertyStore(ParentShellObject);
        }
        catch
        {
            # 如果出现异常，释放父级 ShellObject 对象
            if (parent != null!)
            {
                parent.Dispose();
            }
            throw;
        }
        finally
        {
            # 确保释放 IPropertyStore 对象的 COM 资源
            if (nativePropertyStore != null)
            {
                Marshal.ReleaseComObject(nativePropertyStore);
                nativePropertyStore = null!;
            }
        }
    }

    # 构造函数：根据路径创建 ShellObject 对象初始化集合
    public ShellPropertyCollection(string path) : this(ShellObjectFactory.Create(path))
    {
    }

    # 内部构造函数：根据 IPropertyStore 实例初始化集合
    internal ShellPropertyCollection(IPropertyStore nativePropertyStore)
        : base(new List<IShellProperty>())
    {
        NativePropertyStore = nativePropertyStore;
    }

    # 析构函数：在对象销毁时进行资源清理
    ~ShellPropertyCollection()
    {
        Dispose(false);
    }

    # 私有属性：存储 IPropertyStore 实例
    private IPropertyStore NativePropertyStore { get; set; }
    # 私有属性：存储父级 ShellObject 对象
    private ShellObject ParentShellObject { get; set; }

    # 实现 IDisposable 接口的 Dispose 方法
    public void Dispose()
    {
        Dispose(true);
        GC.SuppressFinalize(this);
    }

    # 内部静态方法：创建默认的 IPropertyStore 实例
    internal static IPropertyStore CreateDefaultPropertyStore(ShellObject shellObj)
    {
        var guid = new Guid(ShellIIDGuid.IPropertyStore);
        var hr = shellObj.NativeShellItem2.GetPropertyStore(
               GetPropertyStoreOptions.BestEffort,
               ref guid,
               out var nativePropertyStore);

        # 如果创建失败，抛出异常
        if (nativePropertyStore == null || !CoreErrorHelper.Succeeded(hr))
        {
            throw new ShellException(hr);
        }

        return nativePropertyStore;
    }

    # 保护虚方法：释放资源
    protected virtual void Dispose(bool disposing)
    {
        if (NativePropertyStore != null)
        {
            Marshal.ReleaseComObject(NativePropertyStore);
            NativePropertyStore = null!;
        }
    }
}
```