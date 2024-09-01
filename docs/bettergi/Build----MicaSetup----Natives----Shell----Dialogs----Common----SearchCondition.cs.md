# `.\better-genshin-impact\Build\MicaSetup\Natives\Shell\Dialogs\Common\SearchCondition.cs`

```cs
# 引入所需的命名空间
﻿using System;
using System.Collections.Generic;
using System.Runtime.InteropServices;

namespace MicaSetup.Shell.Dialogs;

# 禁用特定警告
#pragma warning disable CS8618

# 定义一个实现了 IDisposable 接口的类 SearchCondition
public class SearchCondition : IDisposable
{
    # 存储规范化名称的私有字段
    private readonly string canonicalName;

    # 存储条件操作的私有字段，默认为隐式操作
    private readonly SearchConditionOperation conditionOperation = SearchConditionOperation.Implicit;

    # 存储条件类型的私有字段，默认为叶节点
    private readonly SearchConditionType conditionType = SearchConditionType.Leaf;

    # 存储一个空的 PropertyKey 实例
    private PropertyKey emptyPropertyKey = new();

    # 存储属性键的私有字段
    private PropertyKey propertyKey;

    # 构造函数，接受一个原生搜索条件对象
    internal SearchCondition(ICondition nativeSearchCondition)
    {
        # 如果原生搜索条件对象为空，则抛出异常
        NativeSearchCondition = nativeSearchCondition
            ?? throw new ArgumentNullException("nativeSearchCondition");

        # 获取搜索条件类型
        var hr = NativeSearchCondition.GetConditionType(out conditionType);

        # 如果获取条件类型失败，则抛出异常
        if (!CoreErrorHelper.Succeeded(hr))
        {
            throw new ShellException(hr);
        }

        # 如果条件类型为叶节点
        if (ConditionType == SearchConditionType.Leaf)
        {
            # 创建一个 PropVariant 对象
            using var propVar = new PropVariant();
            # 获取比较信息，包括规范化名称、条件操作和属性值
            hr = NativeSearchCondition.GetComparisonInfo(out canonicalName, out conditionOperation, propVar);

            # 如果获取比较信息失败，则抛出异常
            if (!CoreErrorHelper.Succeeded(hr))
            {
                throw new ShellException(hr);
            }

            # 将属性值转换为字符串并赋值
            PropertyValue = propVar.Value.ToString();
        }
    }

    # 析构函数，调用 Dispose 方法
    ~SearchCondition()
    {
        Dispose(false);
    }

    # 公开条件操作属性
    public SearchConditionOperation ConditionOperation => conditionOperation;

    # 公开条件类型属性
    public SearchConditionType ConditionType => conditionType;

    # 公开属性规范化名称属性
    public string PropertyCanonicalName => canonicalName;

    # 公开属性键属性，如果未初始化则进行初始化
    public PropertyKey PropertyKey
    {
        get
        {
            # 如果属性键是空的，则通过名称获取属性键
            if (propertyKey == emptyPropertyKey)
            {
                var hr = PropertySystemNativeMethods.PSGetPropertyKeyFromName(PropertyCanonicalName, out propertyKey);
                # 如果获取属性键失败，则抛出异常
                if (!CoreErrorHelper.Succeeded(hr))
                {
                    throw new ShellException(hr);
                }
            }
            # 返回属性键
            return propertyKey;
        }
    }

    # 公开属性值
    public string PropertyValue { get; internal set; }

    # 内部属性，存储原生搜索条件对象
    internal ICondition NativeSearchCondition { get; set; }

    # 实现 IDisposable 接口的方法，释放资源
    public void Dispose()
    {
        Dispose(true);
        # 阻止垃圾回收器调用析构函数
        GC.SuppressFinalize(this);
    }

    # 获取子条件的方法
    public IEnumerable<SearchCondition> GetSubConditions()
    {
        // 创建一个空的 SearchCondition 对象列表
        var subConditionsList = new List<SearchCondition>();

        // 使用 ShellIIDGuid.IEnumUnknown 初始化 GUID
        var guid = new Guid(ShellIIDGuid.IEnumUnknown);

        // 调用 NativeSearchCondition 的 GetSubConditions 方法，传入 GUID 并获取子条件对象
        var hr = NativeSearchCondition.GetSubConditions(ref guid, out var subConditionObj);

        // 检查调用是否成功，如果不成功则抛出异常
        if (!CoreErrorHelper.Succeeded(hr))
        {
            throw new ShellException(hr);
        }

        // 如果 subConditionObj 不为空，进行进一步处理
        if (subConditionObj != null)
        {
            // 将 subConditionObj 转换为 IEnumUnknown 接口类型
            var enumUnknown = subConditionObj as IEnumUnknown;

            // 定义用于存储数据的指针和用于存储取出元素数量的变量
            nint buffer = 0;
            uint fetched = 0;

            // 循环获取子条件直到没有更多数据
            while (hr == HResult.Ok)
            {
                // 从枚举中获取下一个元素
                hr = enumUnknown!.Next(1, ref buffer, ref fetched);

                // 如果获取成功且取出的元素数量为 1，则将其转换为 SearchCondition 并添加到列表中
                if (hr == HResult.Ok && fetched == 1)
                {
                    subConditionsList.Add(new SearchCondition((ICondition)Marshal.GetObjectForIUnknown(buffer)));
                }
            }
        }

        // 返回包含子条件的列表
        return subConditionsList;
    }

    // 处理对象的释放，确保释放 COM 对象
    protected virtual void Dispose(bool disposing)
    {
        // 如果 NativeSearchCondition 不为空，则释放其 COM 对象并将其设为 null
        if (NativeSearchCondition != null)
        {
            Marshal.ReleaseComObject(NativeSearchCondition);
            NativeSearchCondition = null!;
        }
    }
请提供更多代码或背景信息，以便我可以为每个语句添加详细注释。
```