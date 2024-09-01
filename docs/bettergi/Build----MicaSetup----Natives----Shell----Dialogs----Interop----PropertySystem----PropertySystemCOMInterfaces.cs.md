# `.\better-genshin-impact\Build\MicaSetup\Natives\Shell\Dialogs\Interop\PropertySystem\PropertySystemCOMInterfaces.cs`

```cs
﻿using System;
using System.Runtime.CompilerServices;
using System.Runtime.InteropServices;

namespace MicaSetup.Shell.Dialogs;

#pragma warning disable CS0108

[ComImport,
Guid(ShellIIDGuid.IPropertyDescription),
InterfaceType(ComInterfaceType.InterfaceIsIUnknown)]
public interface IPropertyDescription
{
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    // 获取属性键
    void GetPropertyKey(out PropertyKey pkey);

    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    // 获取属性的规范名称
    void GetCanonicalName([MarshalAs(UnmanagedType.LPWStr)] out string ppszName);

    [PreserveSig]
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    // 获取属性的类型
    HResult GetPropertyType(out VarEnum pvartype);

    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime),
    PreserveSig]
    // 获取属性的显示名称
    HResult GetDisplayName(out nint ppszName);

    [PreserveSig]
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    // 获取属性的编辑提示
    HResult GetEditInvitation(out nint ppszInvite);

    [PreserveSig]
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    // 获取属性类型标志
    HResult GetTypeFlags([In] PropertyTypeOptions mask, out PropertyTypeOptions ppdtFlags);

    [PreserveSig]
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    // 获取属性的视图标志
    HResult GetViewFlags(out PropertyViewOptions ppdvFlags);

    [PreserveSig]
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    // 获取属性的默认列宽
    HResult GetDefaultColumnWidth(out uint pcxChars);

    [PreserveSig]
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    // 获取属性的显示类型
    HResult GetDisplayType(out PropertyDisplayType pdisplaytype);

    [PreserveSig]
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    // 获取属性列状态
    HResult GetColumnState(out PropertyColumnStateOptions pcsFlags);

    [PreserveSig]
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    // 获取属性的分组范围
    HResult GetGroupingRange(out PropertyGroupingRange pgr);

    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    // 获取相对描述类型
    void GetRelativeDescriptionType(out PropertySystemNativeMethods.RelativeDescriptionType prdt);

    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    // 获取相对描述
    void GetRelativeDescription([In] PropVariant propvar1, [In] PropVariant propvar2, [MarshalAs(UnmanagedType.LPWStr)] out string ppszDesc1, [MarshalAs(UnmanagedType.LPWStr)] out string ppszDesc2);

    [PreserveSig]
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    // 获取排序描述
    HResult GetSortDescription(out PropertySortDescription psd);

    [PreserveSig]
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    // 获取排序描述标签
    HResult GetSortDescriptionLabel([In] bool fDescending, out nint ppszDescription);
    # 保留方法签名的特性
        [PreserveSig]
        # 指定方法实现为内部调用，并使用运行时代码类型
        [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
        # 获取聚合类型，返回结果为 HResult，聚合类型通过参数 paggtype 输出
        HResult GetAggregationType(out PropertyAggregationType paggtype);
    
        # 保留方法签名的特性
        [PreserveSig]
        # 指定方法实现为内部调用，并使用运行时代码类型
        [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
        # 获取条件类型和默认操作，返回结果为 HResult，类型通过参数 pcontype 和 popDefault 输出
        HResult GetConditionType(out PropertyConditionType pcontype, out PropertyConditionOperation popDefault);
    
        # 保留方法签名的特性
        [PreserveSig]
        # 指定方法实现为内部调用，并使用运行时代码类型
        [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
        # 获取枚举类型列表，使用 Guid 类型的参数 riid 和 IPropertyEnumTypeList 类型的参数 ppv
        HResult GetEnumTypeList([In] ref Guid riid, [Out, MarshalAs(UnmanagedType.Interface)] out IPropertyEnumTypeList ppv);
    
        # 指定方法实现为内部调用，并使用运行时代码类型
        [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
        # 将属性变量转换为规范值，没有返回值
        void CoerceToCanonicalValue([In, Out] PropVariant propvar);
    
        # 保留方法签名的特性
        [PreserveSig]
        # 指定方法实现为内部调用，并使用运行时代码类型
        [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
        # 格式化显示属性变量，结果为 HResult，显示字符串通过 ppszDisplay 输出
        HResult FormatForDisplay([In] PropVariant propvar, [In] ref PropertyDescriptionFormatOptions pdfFlags, [MarshalAs(UnmanagedType.LPWStr)] out string ppszDisplay);
    
        # 指定方法实现为内部调用，并使用运行时代码类型
        [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
        # 判断属性变量是否为规范值，返回结果为 HResult
        HResult IsValueCanonical([In] PropVariant propvar);
}
结束接口定义

[ComImport,
Guid(ShellIIDGuid.IPropertyDescription2),
InterfaceType(ComInterfaceType.InterfaceIsIUnknown)]
标记该接口为 COM 导入接口，指定唯一标识符和接口类型
public interface IPropertyDescription2 : IPropertyDescription
{
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    指定方法在运行时由 COM 调用
    void GetPropertyKey(out PropertyKey pkey);
    获取属性键并通过输出参数返回

    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    void GetCanonicalName([MarshalAs(UnmanagedType.LPWStr)] out string ppszName);
    获取属性的规范名称并通过输出参数返回

    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    void GetPropertyType(out VarEnum pvartype);
    获取属性的数据类型并通过输出参数返回

    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    void GetDisplayName([MarshalAs(UnmanagedType.LPWStr)] out string ppszName);
    获取属性的显示名称并通过输出参数返回

    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    void GetEditInvitation([MarshalAs(UnmanagedType.LPWStr)] out string ppszInvite);
    获取编辑属性时的提示信息并通过输出参数返回

    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    void GetTypeFlags([In] PropertyTypeOptions mask, out PropertyTypeOptions ppdtFlags);
    获取属性类型标志，并通过输出参数返回，mask 参数用于指定类型标志的掩码

    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    void GetViewFlags(out PropertyViewOptions ppdvFlags);
    获取属性视图标志并通过输出参数返回

    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    void GetDefaultColumnWidth(out uint pcxChars);
    获取属性列的默认宽度（以字符为单位）并通过输出参数返回

    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    void GetDisplayType(out PropertyDisplayType pdisplaytype);
    获取属性的显示类型并通过输出参数返回

    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    void GetColumnState(out uint pcsFlags);
    获取属性列的状态标志并通过输出参数返回

    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    void GetGroupingRange(out PropertyGroupingRange pgr);
    获取属性的分组范围并通过输出参数返回

    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    void GetRelativeDescriptionType(out PropertySystemNativeMethods.RelativeDescriptionType prdt);
    获取属性的相对描述类型并通过输出参数返回

    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    void GetRelativeDescription(
       [In] PropVariant propvar1,
       [In] PropVariant propvar2,
       [MarshalAs(UnmanagedType.LPWStr)] out string ppszDesc1,
       [MarshalAs(UnmanagedType.LPWStr)] out string ppszDesc2);
    获取属性的相对描述信息，通过输出参数返回描述字符串，propvar1 和 propvar2 是输入参数

    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    void GetSortDescription(out PropertySortDescription psd);
    获取属性的排序描述并通过输出参数返回

    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    void GetSortDescriptionLabel([In] int fDescending, [MarshalAs(UnmanagedType.LPWStr)] out string ppszDescription);
    获取排序描述标签并通过输出参数返回，fDescending 参数指定是否降序

    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    void GetAggregationType(out PropertyAggregationType paggtype);
    获取属性的聚合类型并通过输出参数返回

    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    void GetAggregationType(out PropertyAggregationType paggtype);
    方法实现的标记和定义，指定在运行时由 COM 调用
    # 获取条件类型和默认操作类型
    void GetConditionType(
        # 输出参数，条件类型
        out PropertyConditionType pcontype,
        # 输出参数，默认操作类型
        out PropertyConditionOperation popDefault);
    
    # 获取枚举类型列表
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    void GetEnumTypeList([In] ref Guid riid, # 输入参数，接口标识符
        out nint ppv); # 输出参数，指向枚举类型列表的指针
    
    # 强制转换为规范值
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    void CoerceToCanonicalValue([In, Out] PropVariant ppropvar); # 输入输出参数，属性值
    
    # 格式化显示
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    void FormatForDisplay([In] PropVariant propvar, # 输入参数，属性值
        [In] ref PropertyDescriptionFormatOptions pdfFlags, # 输入参数，格式化选项
        [MarshalAs(UnmanagedType.LPWStr)] out string ppszDisplay); # 输出参数，格式化后的显示字符串
    
    # 判断值是否为规范值
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    HResult IsValueCanonical([In] PropVariant propvar); # 输入参数，属性值
    
    # 获取值的图像引用
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    void GetImageReferenceForValue(
        [In] PropVariant propvar, # 输入参数，属性值
        [Out, MarshalAs(UnmanagedType.LPWStr)] out string ppszImageRes); # 输出参数，图像资源的引用字符串
}
# 结束之前的接口定义

[ComImport,
# 标记该接口为 COM 接口，指定其 GUID 和接口类型
Guid(ShellIIDGuid.IPropertyDescriptionList),
InterfaceType(ComInterfaceType.InterfaceIsIUnknown)]
public interface IPropertyDescriptionList
{
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    # 定义一个方法，获取属性描述列表中的元素数量
    void GetCount(out uint pcElem);

    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    # 定义一个方法，根据索引获取特定属性描述
    void GetAt([In] uint iElem, [In] ref Guid riid, [MarshalAs(UnmanagedType.Interface)] out IPropertyDescription ppv);
}

[ComImport,
# 标记该接口为 COM 接口，指定其 GUID 和接口类型
Guid(ShellIIDGuid.IPropertyEnumType),
InterfaceType(ComInterfaceType.InterfaceIsIUnknown)]
public interface IPropertyEnumType
{
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    # 定义一个方法，获取枚举类型
    void GetEnumType([Out] out PropEnumType penumtype);

    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    # 定义一个方法，获取属性值
    void GetValue([Out] PropVariant ppropvar);

    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    # 定义一个方法，获取属性的最小值
    void GetRangeMinValue([Out] PropVariant ppropvar);

    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    # 定义一个方法，获取属性的设定值范围
    void GetRangeSetValue([Out] PropVariant ppropvar);

    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    # 定义一个方法，获取显示文本
    void GetDisplayText([Out, MarshalAs(UnmanagedType.LPWStr)] out string ppszDisplay);
}

[ComImport,
# 标记该接口为 COM 接口，指定其 GUID 和接口类型
Guid(ShellIIDGuid.IPropertyEnumType2),
InterfaceType(ComInterfaceType.InterfaceIsIUnknown)]
public interface IPropertyEnumType2 : IPropertyEnumType
{
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    # 定义一个方法，获取枚举类型，继承自 IPropertyEnumType
    void GetEnumType([Out] out PropEnumType penumtype);

    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    # 定义一个方法，获取属性值，继承自 IPropertyEnumType
    void GetValue([Out] PropVariant ppropvar);

    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    # 定义一个方法，获取属性的最小值，继承自 IPropertyEnumType
    void GetRangeMinValue([Out] PropVariant ppropvar);

    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    # 定义一个方法，获取属性的设定值范围，继承自 IPropertyEnumType
    void GetRangeSetValue([Out] PropVariant ppropvar);

    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    # 定义一个方法，获取显示文本，继承自 IPropertyEnumType
    void GetDisplayText([Out, MarshalAs(UnmanagedType.LPWStr)] out string ppszDisplay);

    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    # 定义一个方法，获取图像资源引用
    void GetImageReference([Out, MarshalAs(UnmanagedType.LPWStr)] out string ppszImageRes);
}

[ComImport,
# 标记该接口为 COM 接口，指定其 GUID 和接口类型
Guid(ShellIIDGuid.IPropertyEnumTypeList),
InterfaceType(ComInterfaceType.InterfaceIsIUnknown)]
public interface IPropertyEnumTypeList
{
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    # 定义一个方法，获取枚举类型的数量
    void GetCount([Out] out uint pctypes);

    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    # 定义一个方法，获取指定索引的枚举类型
    void GetAt(
    [In] uint itype,
    [In] ref Guid riid,
    # 输出参数，指定接口类型的属性枚举类型
    [Out, MarshalAs(UnmanagedType.Interface)] out IPropertyEnumType ppv);

    # 使用内部调用方式定义的方法，用于获取指定索引处的条件
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    void GetConditionAt(
    # 输入参数，指定要获取的条件索引
    [In] uint index,
    # 输入参数，指定 GUID 类型的引用，用于接口标识
    [In] ref Guid riid,
    # 输出参数，指定获取的条件对象
    out nint ppv);

    # 使用内部调用方式定义的方法，用于查找与给定属性变量匹配的索引
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    void FindMatchingIndex(
    # 输入参数，指定要匹配的属性变量
    [In] PropVariant propvarCmp,
    # 输出参数，指定找到的匹配索引
    [Out] out uint pnIndex);
# 结束上一个代码块

[ComImport] # 表示该接口是一个 COM 组件接口，需要通过 COM 进行调用
[Guid(ShellIIDGuid.IPropertyStore)] # 指定接口的 GUID
[InterfaceType(ComInterfaceType.InterfaceIsIUnknown)] # 指定接口类型为 IUnknown
public interface IPropertyStore
{
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)] # 指定方法实现为内部调用，运行时代码类型
    HResult GetCount([Out] out uint propertyCount); # 获取属性数量

    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)] # 指定方法实现为内部调用，运行时代码类型
    HResult GetAt([In] uint propertyIndex, out PropertyKey key); # 获取指定索引处的属性键

    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)] # 指定方法实现为内部调用，运行时代码类型
    HResult GetValue([In] ref PropertyKey key, [Out] PropVariant pv); # 获取属性的值

    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime), PreserveSig] # 指定方法实现为内部调用，运行时代码类型，并保留方法签名
    HResult SetValue([In] ref PropertyKey key, [In] PropVariant pv); # 设置属性的值

    [PreserveSig] # 保留方法签名
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)] # 指定方法实现为内部调用，运行时代码类型
    HResult Commit(); # 提交更改
}

[ComImport] # 表示该接口是一个 COM 组件接口，需要通过 COM 进行调用
[Guid(ShellIIDGuid.IPropertyStoreCache)] # 指定接口的 GUID
[InterfaceType(ComInterfaceType.InterfaceIsIUnknown)] # 指定接口类型为 IUnknown
public interface IPropertyStoreCache
{
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)] # 指定方法实现为内部调用，运行时代码类型
    HResult GetState(ref PropertyKey key, [Out] out PropertyStoreCacheState state); # 获取属性的缓存状态

    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)] # 指定方法实现为内部调用，运行时代码类型
    HResult GetValueAndState(ref PropertyKey propKey, [Out] PropVariant pv, [Out] out PropertyStoreCacheState state); # 获取属性值和缓存状态

    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)] # 指定方法实现为内部调用，运行时代码类型
    HResult SetState(ref PropertyKey propKey, PropertyStoreCacheState state); # 设置属性的缓存状态

    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)] # 指定方法实现为内部调用，运行时代码类型
    HResult SetValueAndState(ref PropertyKey propKey, [In] PropVariant pv, PropertyStoreCacheState state); # 设置属性值和缓存状态
}

[ComImport] # 表示该接口是一个 COM 组件接口，需要通过 COM 进行调用
[Guid(ShellIIDGuid.IPropertyStoreCapabilities)] # 指定接口的 GUID
[InterfaceType(ComInterfaceType.InterfaceIsIUnknown)] # 指定接口类型为 IUnknown
public interface IPropertyStoreCapabilities
{
    HResult IsPropertyWritable([In] ref PropertyKey propertyKey); # 检查属性是否可写
}

#pragma warning restore 108 # 恢复警告 108 的设置
```