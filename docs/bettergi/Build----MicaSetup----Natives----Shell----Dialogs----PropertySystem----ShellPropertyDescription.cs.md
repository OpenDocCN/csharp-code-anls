# `.\better-genshin-impact\Build\MicaSetup\Natives\Shell\Dialogs\PropertySystem\ShellPropertyDescription.cs`

```cs
# 声明一个命名空间和类，并定义它们的属性和方法
﻿using System;
using System.Collections.Generic;
using System.Collections.ObjectModel;
using System.Runtime.InteropServices;

namespace MicaSetup.Shell.Dialogs;

# 禁用特定的警告
#pragma warning disable CS8618

# 定义 ShellPropertyDescription 类，并实现 IDisposable 接口
public class ShellPropertyDescription : IDisposable
{
    # 声明私有字段以保存属性数据
    private PropertyAggregationType? aggregationTypes;
    private string canonicalName;
    private PropertyColumnStateOptions? columnState;
    private PropertyConditionOperation? conditionOperation;
    private PropertyConditionType? conditionType;
    private uint? defaultColumWidth;
    private string displayName;
    private PropertyDisplayType? displayType;
    private string editInvitation;
    private PropertyGroupingRange? groupingRange;
    private IPropertyDescription nativePropertyDescription;
    private ReadOnlyCollection<ShellPropertyEnumType> propertyEnumTypes;
    private PropertyKey propertyKey;
    private PropertyTypeOptions? propertyTypeFlags;
    private PropertyViewOptions? propertyViewFlags;
    private PropertySortDescription? sortDescription;
    private Type valueType;
    private VarEnum? varEnumType = null;

    # 使用构造函数初始化属性键
    internal ShellPropertyDescription(PropertyKey key) => propertyKey = key;

    # 析构函数，调用 Dispose 方法释放资源
    ~ShellPropertyDescription()
    {
        Dispose(false);
    }

    # 属性 AggregationTypes，返回聚合类型
    public PropertyAggregationType AggregationTypes
    {
        get
        {
            # 如果 NativePropertyDescription 不为 null 且 aggregationTypes 为 null，则获取聚合类型
            if (NativePropertyDescription != null && aggregationTypes == null)
            {
                var hr = NativePropertyDescription.GetAggregationType(out var tempAggregationTypes);

                # 如果获取聚合类型成功，则保存结果
                if (CoreErrorHelper.Succeeded(hr))
                {
                    aggregationTypes = tempAggregationTypes;
                }
            }

            # 返回 aggregationTypes 的值或默认值
            return aggregationTypes.HasValue ? aggregationTypes.Value : default(PropertyAggregationType);
        }
    }

    # 属性 CanonicalName，返回规范名称
    public string CanonicalName
    {
        get
        {
            # 如果 canonicalName 为 null，则从属性键获取规范名称
            if (canonicalName == null)
            {
                PropertySystemNativeMethods.PSGetNameFromPropertyKey(ref propertyKey, out canonicalName);
            }

            # 返回 canonicalName
            return canonicalName;
        }
    }

    # 属性 ColumnState，返回列状态
    public PropertyColumnStateOptions ColumnState
    {
        get
        {
            # 如果 NativePropertyDescription 不为 null 且 columnState 为 null，则获取列状态
            if (NativePropertyDescription != null && columnState == null)
            {
                var hr = NativePropertyDescription.GetColumnState(out var state);

                # 如果获取列状态成功，则保存结果
                if (CoreErrorHelper.Succeeded(hr))
                {
                    columnState = state;
                }
            }

            # 返回 columnState 的值或默认值
            return columnState.HasValue ? columnState.Value : default(PropertyColumnStateOptions);
        }
    }

    # 属性 ConditionOperation，返回条件操作
    public PropertyConditionOperation ConditionOperation
    {
        get
        {
            # 如果 NativePropertyDescription 不为空且 conditionOperation 为 null
            if (NativePropertyDescription != null && conditionOperation == null)
            {
                # 调用 GetConditionType 方法获取条件类型和操作
                var hr = NativePropertyDescription.GetConditionType(out var tempConditionType, out var tempConditionOperation);

                # 如果方法调用成功
                if (CoreErrorHelper.Succeeded(hr))
                {
                    # 设置条件操作和条件类型
                    conditionOperation = tempConditionOperation;
                    conditionType = tempConditionType;
                }
            }

            # 返回条件操作的值，如果没有值则返回默认值
            return conditionOperation.HasValue ? conditionOperation.Value : default(PropertyConditionOperation);
        }
    }

    public PropertyConditionType ConditionType
    {
        get
        {
            # 如果 NativePropertyDescription 不为空且 conditionType 为 null
            if (NativePropertyDescription != null && conditionType == null)
            {
                # 调用 GetConditionType 方法获取条件类型和操作
                var hr = NativePropertyDescription.GetConditionType(out var tempConditionType, out var tempConditionOperation);

                # 如果方法调用成功
                if (CoreErrorHelper.Succeeded(hr))
                {
                    # 设置条件操作和条件类型
                    conditionOperation = tempConditionOperation;
                    conditionType = tempConditionType;
                }
            }

            # 返回条件类型的值，如果没有值则返回默认值
            return conditionType.HasValue ? conditionType.Value : default(PropertyConditionType);
        }
    }

    public uint DefaultColumWidth
    {
        get
        {
            # 如果 NativePropertyDescription 不为空且 defaultColumWidth 为 null
            if (NativePropertyDescription != null && !defaultColumWidth.HasValue)
            {
                # 调用 GetDefaultColumnWidth 方法获取默认列宽
                var hr = NativePropertyDescription.GetDefaultColumnWidth(out var tempDefaultColumWidth);

                # 如果方法调用成功
                if (CoreErrorHelper.Succeeded(hr))
                {
                    # 设置默认列宽
                    defaultColumWidth = tempDefaultColumWidth;
                }
            }

            # 返回默认列宽的值，如果没有值则返回默认值
            return defaultColumWidth.HasValue ? defaultColumWidth.Value : default(uint);
        }
    }

    public string DisplayName
    {
        get
        {
            # 如果 NativePropertyDescription 不为空且 displayName 为 null
            if (NativePropertyDescription != null && displayName == null)
            {
                # 调用 GetDisplayName 方法获取显示名称
                var hr = NativePropertyDescription.GetDisplayName(out nint dispNameptr);

                # 如果方法调用成功且指针不为 0
                if (CoreErrorHelper.Succeeded(hr) && dispNameptr != 0)
                {
                    # 从指针转换为字符串并设置 displayName
                    displayName = Marshal.PtrToStringUni(dispNameptr);

                    # 释放分配的内存
                    Marshal.FreeCoTaskMem(dispNameptr);
                }
            }

            # 返回 displayName 的值
            return displayName!;
        }
    }

    public PropertyDisplayType DisplayType
    {
        get
        {
            # 如果 NativePropertyDescription 不为空且 displayType 为 null
            if (NativePropertyDescription != null && displayType == null)
            {
                # 调用 GetDisplayType 方法获取显示类型
                var hr = NativePropertyDescription.GetDisplayType(out var tempDisplayType);

                # 如果方法调用成功
                if (CoreErrorHelper.Succeeded(hr))
                {
                    # 设置显示类型
                    displayType = tempDisplayType;
                }
            }

            # 返回显示类型的值，如果没有值则返回默认值
            return displayType.HasValue ? displayType.Value : default(PropertyDisplayType);
        }
    }

    public string EditInvitation
    {
        get
        {
            # 如果 NativePropertyDescription 不为空且 editInvitation 为空，则执行以下代码
            if (NativePropertyDescription != null && editInvitation == null)
            {
                # 调用 NativePropertyDescription 的 GetEditInvitation 方法，并将结果指针赋值给 ptr
                var hr = NativePropertyDescription.GetEditInvitation(out nint ptr);

                # 如果操作成功且指针非零，则继续处理
                if (CoreErrorHelper.Succeeded(hr) && ptr != 0)
                {
                    # 将指针转换为字符串，并赋值给 editInvitation
                    editInvitation = Marshal.PtrToStringUni(ptr);
                    # 释放指针所占的内存
                    Marshal.FreeCoTaskMem(ptr);
                }
            }

            # 返回 editInvitation 的值
            return editInvitation!;
        }
    }

    public PropertyGroupingRange GroupingRange
    {
        get
        {
            # 如果 NativePropertyDescription 不为空且 groupingRange 为空，则执行以下代码
            if (NativePropertyDescription != null && groupingRange == null)
            {
                # 调用 NativePropertyDescription 的 GetGroupingRange 方法，并将结果赋值给 tempGroupingRange
                var hr = NativePropertyDescription.GetGroupingRange(out var tempGroupingRange);

                # 如果操作成功，则将 tempGroupingRange 赋值给 groupingRange
                if (CoreErrorHelper.Succeeded(hr))
                {
                    groupingRange = tempGroupingRange;
                }
            }

            # 返回 groupingRange 的值，如果为空则返回默认值
            return groupingRange.HasValue ? groupingRange.Value : default(PropertyGroupingRange);
        }
    }

    public bool HasSystemDescription => NativePropertyDescription != null;

    public ReadOnlyCollection<ShellPropertyEnumType> PropertyEnumTypes
    {
        get
        {
            # 如果 NativePropertyDescription 不为空且 propertyEnumTypes 为空，则执行以下代码
            if (NativePropertyDescription != null && propertyEnumTypes == null)
            {
                # 创建一个新的 ShellPropertyEnumType 列表
                var propEnumTypeList = new List<ShellPropertyEnumType>();

                # 创建一个 GUID 实例，表示 IPropertyEnumTypeList
                var guid = new Guid(ShellIIDGuid.IPropertyEnumTypeList);
                # 调用 NativePropertyDescription 的 GetEnumTypeList 方法，并将结果赋值给 nativeList
                var hr = NativePropertyDescription.GetEnumTypeList(ref guid, out var nativeList);

                # 如果 nativeList 不为空且操作成功，则继续处理
                if (nativeList != null && CoreErrorHelper.Succeeded(hr))
                {
                    # 获取 nativeList 的项数
                    nativeList.GetCount(out var count);
                    # 创建一个 GUID 实例，表示 IPropertyEnumType
                    guid = new Guid(ShellIIDGuid.IPropertyEnumType);

                    # 遍历每个项，并将其添加到 propEnumTypeList
                    for (uint i = 0; i < count; i++)
                    {
                        nativeList.GetAt(i, ref guid, out var nativeEnumType);
                        propEnumTypeList.Add(new ShellPropertyEnumType(nativeEnumType));
                    }
                }

                # 将 propEnumTypeList 转换为只读集合，并赋值给 propertyEnumTypes
                propertyEnumTypes = new ReadOnlyCollection<ShellPropertyEnumType>(propEnumTypeList);
            }

            # 返回 propertyEnumTypes 的值
            return propertyEnumTypes;
        }
    }

    public PropertyKey PropertyKey => propertyKey;

    public PropertySortDescription SortDescription
    {
        get
        {
            # 如果 NativePropertyDescription 不为空且 sortDescription 为空，则执行以下代码
            if (NativePropertyDescription != null && sortDescription == null)
            {
                # 调用 NativePropertyDescription 的 GetSortDescription 方法，并将结果赋值给 tempSortDescription
                var hr = NativePropertyDescription.GetSortDescription(out var tempSortDescription);

                # 如果操作成功，则将 tempSortDescription 赋值给 sortDescription
                if (CoreErrorHelper.Succeeded(hr))
                {
                    sortDescription = tempSortDescription;
                }
            }

            # 返回 sortDescription 的值，如果为空则返回默认值
            return sortDescription.HasValue ? sortDescription.Value : default(PropertySortDescription);
        }
    }

    public PropertyTypeOptions TypeFlags
    {
        get
        {
            # 检查是否已初始化且 propertyTypeFlags 为 null
            if (NativePropertyDescription != null && propertyTypeFlags == null)
            {
                # 从 NativePropertyDescription 获取属性类型标志
                var hr = NativePropertyDescription.GetTypeFlags(PropertyTypeOptions.MaskAll, out var tempFlags);

                # 根据 HRESULT 判断操作是否成功，若成功则设置 propertyTypeFlags，否则使用默认值
                propertyTypeFlags = CoreErrorHelper.Succeeded(hr) ? tempFlags : default(PropertyTypeOptions);
            }

            # 返回 propertyTypeFlags 的值，如果为 null，则返回默认值
            return propertyTypeFlags.HasValue ? propertyTypeFlags.Value : default(PropertyTypeOptions);
        }
    }

    public Type ValueType
    {
        get
        {
            # 检查 valueType 是否为 null
            if (valueType == null)
            {
                # 使用 ShellPropertyFactory 将 VarEnumType 转换为系统类型
                valueType = ShellPropertyFactory.VarEnumToSystemType(VarEnumType);
            }

            # 返回 valueType 的值
            return valueType;
        }
    }

    public VarEnum VarEnumType
    {
        get
        {
            # 检查 NativePropertyDescription 是否已初始化且 varEnumType 为 null
            if (NativePropertyDescription != null && varEnumType == null)
            {
                # 从 NativePropertyDescription 获取属性类型
                var hr = NativePropertyDescription.GetPropertyType(out var tempType);

                # 根据 HRESULT 判断操作是否成功，若成功则设置 varEnumType
                if (CoreErrorHelper.Succeeded(hr))
                {
                    varEnumType = tempType;
                }
            }

            # 返回 varEnumType 的值，如果为 null，则返回默认值
            return varEnumType.HasValue ? varEnumType.Value : default(VarEnum);
        }
    }

    public PropertyViewOptions ViewFlags
    {
        get
        {
            # 检查 NativePropertyDescription 是否已初始化且 propertyViewFlags 为 null
            if (NativePropertyDescription != null && propertyViewFlags == null)
            {
                # 从 NativePropertyDescription 获取视图标志
                var hr = NativePropertyDescription.GetViewFlags(out var tempFlags);

                # 根据 HRESULT 判断操作是否成功，若成功则设置 propertyViewFlags，否则使用默认值
                propertyViewFlags = CoreErrorHelper.Succeeded(hr) ? tempFlags : default(PropertyViewOptions);
            }

            # 返回 propertyViewFlags 的值，如果为 null，则返回默认值
            return propertyViewFlags.HasValue ? propertyViewFlags.Value : default(PropertyViewOptions);
        }
    }

    internal IPropertyDescription NativePropertyDescription
    {
        get
        {
            # 检查 nativePropertyDescription 是否为 null
            if (nativePropertyDescription == null)
            {
                # 初始化 GUID 并调用方法获取属性描述
                var guid = new Guid(ShellIIDGuid.IPropertyDescription);
                PropertySystemNativeMethods.PSGetPropertyDescription(ref propertyKey, ref guid, out nativePropertyDescription);
            }

            # 返回 nativePropertyDescription
            return nativePropertyDescription;
        }
    }

    public void Dispose()
    {
        # 调用带有 disposing 参数的 Dispose 方法
        Dispose(true);
        # 防止垃圾回收器再次调用析构函数
        GC.SuppressFinalize(this);
    }

    public string GetSortDescriptionLabel(bool descending)
    {
        # 初始化空字符串 label
        var label = string.Empty;

        # 检查 NativePropertyDescription 是否已初始化
        if (NativePropertyDescription != null)
        {
            # 从 NativePropertyDescription 获取排序描述标签
            var hr = NativePropertyDescription.GetSortDescriptionLabel(descending, out nint ptr);

            # 根据 HRESULT 判断操作是否成功且指针有效
            if (CoreErrorHelper.Succeeded(hr) && ptr != 0)
            {
                # 从指针获取字符串并释放内存
                label = Marshal.PtrToStringUni(ptr);
                Marshal.FreeCoTaskMem(ptr);
            }
        }

        # 返回排序描述标签
        return label;
    }

    protected virtual void Dispose(bool disposing)
    {
        # 检查 nativePropertyDescription 是否不为空
        if (nativePropertyDescription != null)
        {
            # 释放 nativePropertyDescription 对象的 COM 资源
            Marshal.ReleaseComObject(nativePropertyDescription);
            # 将 nativePropertyDescription 设置为 null，表示对象不再有效
            nativePropertyDescription = null!;
        }

        # 如果 disposing 为真，执行清理操作
        if (disposing)
        {
            # 将 canonicalName 设置为 null，释放该资源
            canonicalName = null!;
            # 将 displayName 设置为 null，释放该资源
            displayName = null!;
            # 将 editInvitation 设置为 null，释放该资源
            editInvitation = null!;
            # 将 defaultColumWidth 设置为 null，释放该资源
            defaultColumWidth = null!;
            # 将 valueType 设置为 null，释放该资源
            valueType = null!;
            # 将 propertyEnumTypes 设置为 null，释放该资源
            propertyEnumTypes = null!;
        }
    }
请提供需要注释的代码块。
```