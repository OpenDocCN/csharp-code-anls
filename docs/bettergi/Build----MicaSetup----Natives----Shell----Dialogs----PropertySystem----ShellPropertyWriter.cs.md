# `.\better-genshin-impact\Build\MicaSetup\Natives\Shell\Dialogs\PropertySystem\ShellPropertyWriter.cs`

```cs
# 命名空间声明
using System;
using System.Runtime.InteropServices;

namespace MicaSetup.Shell.Dialogs;

#pragma warning disable CS8618 # 禁用特定的警告

# 定义一个实现 IDisposable 接口的类
public class ShellPropertyWriter : IDisposable
{
    # 内部属性存储可写的属性存储
    internal IPropertyStore writablePropStore;

    # 存储父 Shell 对象
    private ShellObject parentShellObject;

    # 构造函数，初始化 ShellPropertyWriter 实例
    internal ShellPropertyWriter(ShellObject parent)
    {
        ParentShellObject = parent;

        # 创建 GUID 实例用于获取属性存储
        var guid = new Guid(ShellIIDGuid.IPropertyStore);

        try
        {
            # 调用 NativeShellItem2 获取可写的属性存储
            var hr = ParentShellObject.NativeShellItem2.GetPropertyStore(
                    GetPropertyStoreOptions.ReadWrite,
                    ref guid,
                    out writablePropStore);

            # 检查获取属性存储是否成功，若失败则抛出异常
            if (!CoreErrorHelper.Succeeded(hr))
            {
                throw new PropertySystemException(LocalizedMessages.ShellPropertyUnableToGetWritableProperty,
                    Marshal.GetExceptionForHR(hr));
            }
            else
            {
                # 如果 NativePropertyStore 为空，则将 writablePropStore 分配给它
                if (ParentShellObject.NativePropertyStore == null)
                {
                    ParentShellObject.NativePropertyStore = writablePropStore;
                }
            }
        }
        catch (InvalidComObjectException e)
        {
            # 捕获 COM 对象异常并抛出自定义异常
            throw new PropertySystemException(LocalizedMessages.ShellPropertyUnableToGetWritableProperty, e);
        }
        catch (InvalidCastException)
        {
            # 捕获类型转换异常并抛出自定义异常
            throw new PropertySystemException(LocalizedMessages.ShellPropertyUnableToGetWritableProperty);
        }
    }

    # 析构函数，调用 Dispose 方法
    ~ShellPropertyWriter()
    {
        Dispose(false);
    }

    # 保护的属性，用于获取和设置父 Shell 对象
    protected ShellObject ParentShellObject
    {
        get => parentShellObject;
        private set => parentShellObject = value;
    }

    # 关闭方法，提交属性存储并释放 COM 对象
    public void Close()
    {
        if (writablePropStore != null)
        {
            writablePropStore.Commit();

            Marshal.ReleaseComObject(writablePropStore);
            writablePropStore = null!;
        }

        ParentShellObject.NativePropertyStore = null!;
    }

    # 实现 IDisposable 接口的方法，处理资源释放
    public void Dispose()
    {
        Dispose(true);
        GC.SuppressFinalize(this);
    }

    # 重载 WriteProperty 方法，调用具体实现方法
    public void WriteProperty(PropertyKey key, object value) => WriteProperty(key, value, true);

    # 重载 WriteProperty 方法，允许截断值的写入
    public void WriteProperty(PropertyKey key, object value, bool allowTruncatedValue)
    {
        # 检查可写属性存储是否为 null，若是则抛出异常
        if (writablePropStore == null)
            throw new InvalidOperationException("Writeable store has been closed.");

        # 将对象转换为 PropVariant 并使用 using 声明以确保资源正确释放
        using var propVar = PropVariant.FromObject(value);
        # 使用键和值设置属性，并获取结果
        var result = writablePropStore.SetValue(ref key, propVar);

        # 如果不允许截断值且结果表明字符串被截断，则释放 COM 对象并抛出异常
        if (!allowTruncatedValue && ((int)result == ShellNativeMethods.InPlaceStringTruncated))
        {
            Marshal.ReleaseComObject(writablePropStore);
            writablePropStore = null!;

            throw new ArgumentOutOfRangeException("value", LocalizedMessages.ShellPropertyValueTruncated);
        }

        # 如果设置属性失败，则抛出异常
        if (!CoreErrorHelper.Succeeded(result))
        {
            throw new PropertySystemException(LocalizedMessages.ShellPropertySetValue, Marshal.GetExceptionForHR((int)result));
        }
    }

    # 使用默认参数调用 WriteProperty 方法
    public void WriteProperty(string canonicalName, object value) => WriteProperty(canonicalName, value, true);

    # 写入属性，允许指定是否接受截断值
    public void WriteProperty(string canonicalName, object value, bool allowTruncatedValue)
    {
        # 从名称获取属性键
        var result = PropertySystemNativeMethods.PSGetPropertyKeyFromName(canonicalName, out var propKey);

        # 如果获取属性键失败，则抛出异常
        if (!CoreErrorHelper.Succeeded(result))
        {
            throw new ArgumentException(
                LocalizedMessages.ShellInvalidCanonicalName,
                Marshal.GetExceptionForHR(result));
        }

        # 调用另一个 WriteProperty 方法，实际写入属性
        WriteProperty(propKey, value, allowTruncatedValue);
    }

    # 使用默认参数调用 WriteProperty 方法
    public void WriteProperty(IShellProperty shellProperty, object value) => WriteProperty(shellProperty, value, true);

    # 写入属性，允许指定是否接受截断值
    public void WriteProperty(IShellProperty shellProperty, object value, bool allowTruncatedValue)
    {
        # 检查 IShellProperty 是否为 null，若是则抛出异常
        if (shellProperty == null) { throw new ArgumentNullException("shellProperty"); }
        # 调用另一个 WriteProperty 方法，实际写入属性
        WriteProperty(shellProperty.PropertyKey, value, allowTruncatedValue);
    }

    # 使用默认参数调用 WriteProperty 方法
    public void WriteProperty<T>(ShellProperty<T> shellProperty, T value) => WriteProperty<T>(shellProperty, value, true);

    # 写入属性，允许指定是否接受截断值
    public void WriteProperty<T>(ShellProperty<T> shellProperty, T value, bool allowTruncatedValue)
    {
        # 检查 ShellProperty 是否为 null，若是则抛出异常
        if (shellProperty == null) { throw new ArgumentNullException("shellProperty"); }
        # 调用另一个 WriteProperty 方法，实际写入属性
        WriteProperty(shellProperty.PropertyKey, value!, allowTruncatedValue);
    }

    # 释放资源的虚方法，调用 Close 方法
    protected virtual void Dispose(bool disposing) => Close();
# 代码块的结束标志
}
```