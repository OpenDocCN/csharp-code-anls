# `.\better-genshin-impact\Build\MicaSetup\Natives\Shell\Dialogs\PropertySystem\ShellProperty.cs`

```cs
﻿using MicaSetup.Helper;  // 引入 MicaSetup.Helper 命名空间
using MicaSetup.Natives;  // 引入 MicaSetup.Natives 命名空间
using System;  // 引入 System 命名空间
using System.Diagnostics;  // 引入 System.Diagnostics 命名空间
using System.Runtime.InteropServices;  // 引入 System.Runtime.InteropServices 命名空间

namespace MicaSetup.Shell.Dialogs;  // 定义 MicaSetup.Shell.Dialogs 命名空间

#pragma warning disable CS8618  // 禁用关于未初始化字段的警告
#pragma warning disable IDE0059  // 禁用关于不必要的赋值警告

public class ShellProperty<T> : IShellProperty  // 定义 ShellProperty 泛型类，实现 IShellProperty 接口
{
    private readonly ShellPropertyDescription description = null!;  // 声明并初始化 ShellPropertyDescription 描述字段
    private int? imageReferenceIconIndex;  // 声明图标索引字段
    private string imageReferencePath = null!;  // 声明并初始化图标路径字段
    private PropertyKey propertyKey;  // 声明 PropertyKey 字段

    internal ShellProperty(  // 定义内部构造函数，接收 PropertyKey、ShellPropertyDescription 和 ShellObject 参数
        PropertyKey propertyKey,
        ShellPropertyDescription description,
        ShellObject parent)
    {
        this.propertyKey = propertyKey;  // 初始化 propertyKey 字段
        this.description = description;  // 初始化 description 字段
        ParentShellObject = parent;  // 初始化 ParentShellObject 字段
        AllowSetTruncatedValue = false;  // 初始化 AllowSetTruncatedValue 属性
    }

    internal ShellProperty(  // 定义另一个内部构造函数，接收 PropertyKey、ShellPropertyDescription 和 IPropertyStore 参数
        PropertyKey propertyKey,
        ShellPropertyDescription description,
        IPropertyStore propertyStore)
    {
        this.propertyKey = propertyKey;  // 初始化 propertyKey 字段
        this.description = description;  // 初始化 description 字段
        NativePropertyStore = propertyStore;  // 初始化 NativePropertyStore 字段
        AllowSetTruncatedValue = false;  // 初始化 AllowSetTruncatedValue 属性
    }

    public bool AllowSetTruncatedValue { get; set; }  // 声明允许设置截断值的属性

    public ShellPropertyDescription Description => description;  // 返回 description 字段

    public IconReference IconReference  // 定义 IconReference 属性
    {
        get
        {
            if (!OsVersionHelper.IsWindows7_OrGreater)  // 如果操作系统版本小于 Windows 7
            {
                throw new PlatformNotSupportedException(LocalizedMessages.ShellPropertyWindows7);  // 抛出不支持的平台异常
            }

            GetImageReference();  // 调用 GetImageReference 方法
            var index = imageReferenceIconIndex ?? -1;  // 如果 imageReferenceIconIndex 为 null，则设置索引为 -1

            return new IconReference(imageReferencePath, index);  // 返回新的 IconReference 对象
        }
    }

    public PropertyKey PropertyKey => propertyKey;  // 返回 propertyKey 字段

    public object ValueAsObject  // 定义 ValueAsObject 属性
    {
        get
        {
            using var propVar = new PropVariant();  // 创建 PropVariant 实例并自动释放
            if (ParentShellObject != null!)  // 如果 ParentShellObject 不为 null
            {
                var store = ShellPropertyCollection.CreateDefaultPropertyStore(ParentShellObject);  // 创建默认属性存储

                store.GetValue(ref propertyKey, propVar);  // 获取属性值并存储在 propVar 中

                Marshal.ReleaseComObject(store);  // 释放 COM 对象
                store = null;  // 将 store 置为 null
            }
            else
            {
                NativePropertyStore?.GetValue(ref propertyKey, propVar);  // 从 NativePropertyStore 获取属性值
            }

            return propVar?.Value!;  // 返回 propVar 的值
        }
    }

    public Type ValueType  // 定义 ValueType 属性
    {
        get
        {
            Debug.Assert(Description.ValueType == typeof(T));  // 确保 Description.ValueType 与 T 类型匹配

            return Description.ValueType;  // 返回 Description.ValueType
        }
    }

    public string CanonicalName => Description.CanonicalName;  // 返回 Description.CanonicalName

    private IPropertyStore NativePropertyStore { get; set; }  // 声明 NativePropertyStore 属性
    private ShellObject ParentShellObject { get; set; }  // 声明 ParentShellObject 属性

    public void ClearValue()  // 定义 ClearValue 方法
    {
        using var propVar = new PropVariant();  // 创建 PropVariant 实例并自动释放
        StorePropVariantValue(propVar);  // 调用 StorePropVariantValue 方法
    }

    public string FormatForDisplay(PropertyDescriptionFormatOptions format)  // 定义 FormatForDisplay 方法
    {
        # 如果 Description 或 Description.NativePropertyDescription 为 null，则返回 null
        if (Description == null || Description.NativePropertyDescription == null)
        {
            return null!;
        }

        # 创建一个默认的属性存储对象，使用 ParentShellObject 作为父对象
        var store = ShellPropertyCollection.CreateDefaultPropertyStore(ParentShellObject);

        # 使用 PropVariant 对象来获取属性值
        using var propVar = new PropVariant();
        # 从属性存储中获取指定属性的值
        store.GetValue(ref propertyKey, propVar);

        # 释放 COM 对象，避免内存泄漏
        Marshal.ReleaseComObject(store);
        store = null;

        # 使用 Description.NativePropertyDescription 对象格式化属性值
        var hr = Description.NativePropertyDescription.FormatForDisplay(propVar, ref format, out var formattedString);

        # 检查格式化操作是否成功，若失败则抛出异常
        if (!CoreErrorHelper.Succeeded(hr))
            throw new ShellException(hr);

        # 返回格式化后的字符串
        return formattedString;
    }

    private void GetImageReference()
    {
        # 创建一个默认的属性存储对象，使用 ParentShellObject 作为父对象
        var store = ShellPropertyCollection.CreateDefaultPropertyStore(ParentShellObject);

        # 使用 PropVariant 对象来获取属性值
        using var propVar = new PropVariant();
        # 从属性存储中获取指定属性的值
        store.GetValue(ref propertyKey, propVar);

        # 释放 COM 对象，避免内存泄漏
        Marshal.ReleaseComObject(store);
        store = null;

        # 从 Description.NativePropertyDescription 获取值的图像引用
        ((IPropertyDescription2)Description.NativePropertyDescription).GetImageReferenceForValue(
            propVar, out var refPath);

        # 如果 refPath 为空，则返回
        if (refPath == null) { return; }

        # 解析图像引用路径中的图标索引
        var index = ShlwApi.PathParseIconLocation(ref refPath);
        # 如果 refPath 不为空，则保存图像引用路径和图标索引
        if (refPath != null)
        {
            imageReferencePath = refPath;
            imageReferenceIconIndex = index;
        }
    }

    private void StorePropVariantValue(PropVariant propVar)
    {
        // 创建一个新的 Guid 对象，用于标识 IPropertyStore
        var guid = new Guid(ShellIIDGuid.IPropertyStore);
        // 初始化可写的属性存储对象为 null
        IPropertyStore writablePropStore = null!;
        try
        {
            // 获取一个可读写的属性存储对象，并传递给 writablePropStore
            var hr = ParentShellObject.NativeShellItem2.GetPropertyStore(
                    GetPropertyStoreOptions.ReadWrite,
                    ref guid,
                    out writablePropStore);

            // 检查是否成功获取属性存储对象
            if (!CoreErrorHelper.Succeeded(hr))
            {
                // 获取失败，抛出属性系统异常
                throw new PropertySystemException(LocalizedMessages.ShellPropertyUnableToGetWritableProperty,
                    Marshal.GetExceptionForHR(hr));
            }

            // 设置属性值
            var result = writablePropStore.SetValue(ref propertyKey, propVar);

            // 如果不允许设置被截断的值且结果指示字符串被截断，则抛出异常
            if (!AllowSetTruncatedValue && (int)result == ShellNativeMethods.InPlaceStringTruncated)
            {
                throw new ArgumentOutOfRangeException("propVar", LocalizedMessages.ShellPropertyValueTruncated);
            }

            // 检查设置属性值是否成功
            if (!CoreErrorHelper.Succeeded(result))
            {
                // 设置失败，抛出属性系统异常
                throw new PropertySystemException(LocalizedMessages.ShellPropertySetValue, Marshal.GetExceptionForHR((int)result));
            }

            // 提交对属性存储对象的更改
            writablePropStore.Commit();
        }
        catch (InvalidComObjectException e)
        {
            // 捕捉无效的 COM 对象异常并抛出属性系统异常
            throw new PropertySystemException(LocalizedMessages.ShellPropertyUnableToGetWritableProperty, e);
        }
        catch (InvalidCastException)
        {
            // 捕捉无效的转换异常并抛出属性系统异常
            throw new PropertySystemException(LocalizedMessages.ShellPropertyUnableToGetWritableProperty);
        }
        finally
        {
            // 确保释放 COM 对象
            if (writablePropStore != null)
            {
                Marshal.ReleaseComObject(writablePropStore);
                writablePropStore = null!;
            }
        }
    }
请提供具体的代码片段以便我进行注释。
```