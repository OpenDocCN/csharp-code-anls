# `.\better-genshin-impact\Build\MicaSetup\Natives\Shell\Dialogs\Common\SearchConditionFactory.cs`

```cs
﻿using System;
using System.Collections.Generic;
using System.Globalization;
using System.Runtime.InteropServices;

namespace MicaSetup.Shell.Dialogs;

public static class SearchConditionFactory
{
    // 创建并/或条件，根据指定的条件类型和简化标志，生成搜索条件对象
    public static SearchCondition CreateAndOrCondition(SearchConditionType conditionType, bool simplify, params SearchCondition[] conditionNodes)
    {
        // 创建一个条件工厂的 COM 对象实例
        var nativeConditionFactory = (IConditionFactory)new ConditionFactoryCoClass();
        // 初始化结果条件对象为 null
        ICondition result = null!;

        try
        {
            // 创建一个条件对象列表
            var conditionList = new List<ICondition>();
            // 如果条件节点不为空，则将每个条件节点的原生条件添加到列表中
            if (conditionNodes != null)
            {
                foreach (var c in conditionNodes)
                {
                    conditionList.Add(c.NativeSearchCondition);
                }
            }

            // 创建一个枚举对象，包含条件列表中的所有条件
            IEnumUnknown subConditions = new EnumUnknownClass(conditionList.ToArray());

            // 调用条件工厂的 MakeAndOr 方法，生成最终的条件对象
            var hr = nativeConditionFactory.MakeAndOr(conditionType, subConditions, simplify, out result);

            // 检查方法调用是否成功，如果不成功，则抛出异常
            if (!CoreErrorHelper.Succeeded(hr)) { throw new ShellException(hr); }
        }
        finally
        {
            // 确保在结束时释放 COM 对象
            if (nativeConditionFactory != null)
            {
                Marshal.ReleaseComObject(nativeConditionFactory);
            }
        }

        // 返回创建的搜索条件对象
        return new SearchCondition(result);
    }

    // 创建基于字符串值的叶子条件
    public static SearchCondition CreateLeafCondition(string propertyName, string value, SearchConditionOperation operation)
    {
        using (var propVar = new PropVariant(value))
        {
            // 调用重载的 CreateLeafCondition 方法创建叶子条件
            return CreateLeafCondition(propertyName, propVar, null!, operation);
        }
    }

    // 创建基于日期值的叶子条件
    public static SearchCondition CreateLeafCondition(string propertyName, DateTime value, SearchConditionOperation operation)
    {
        using (var propVar = new PropVariant(value))
        {
            // 调用重载的 CreateLeafCondition 方法创建叶子条件，指定日期类型
            return CreateLeafCondition(propertyName, propVar, "System.StructuredQuery.CustomProperty.DateTime", operation);
        }
    }

    // 创建基于整数值的叶子条件
    public static SearchCondition CreateLeafCondition(string propertyName, int value, SearchConditionOperation operation)
    {
        using (var propVar = new PropVariant(value))
        {
            // 调用重载的 CreateLeafCondition 方法创建叶子条件，指定整数类型
            return CreateLeafCondition(propertyName, propVar, "System.StructuredQuery.CustomProperty.Integer", operation);
        }
    }

    // 创建基于布尔值的叶子条件
    public static SearchCondition CreateLeafCondition(string propertyName, bool value, SearchConditionOperation operation)
    {
        using (var propVar = new PropVariant(value))
        {
            // 调用重载的 CreateLeafCondition 方法创建叶子条件，指定布尔类型
            return CreateLeafCondition(propertyName, propVar, "System.StructuredQuery.CustomProperty.Boolean", operation);
        }
    }

    // 创建基于浮点值的叶子条件
    public static SearchCondition CreateLeafCondition(string propertyName, double value, SearchConditionOperation operation)
    {
        using (var propVar = new PropVariant(value))
        {
            // 调用重载的 CreateLeafCondition 方法创建叶子条件，指定浮点类型
            return CreateLeafCondition(propertyName, propVar, "System.StructuredQuery.CustomProperty.FloatingPoint", operation);
        }
    }
}
    # 创建一个搜索条件，使用属性键、字符串值和操作符
    public static SearchCondition CreateLeafCondition(PropertyKey propertyKey, string value, SearchConditionOperation operation)
    {
        # 从属性键中获取标准化的属性名称
        PropertySystemNativeMethods.PSGetNameFromPropertyKey(ref propertyKey, out var canonicalName);

        # 检查属性名称是否为空或null，如果是则抛出异常
        if (string.IsNullOrEmpty(canonicalName))
        {
            throw new ArgumentException(LocalizedMessages.SearchConditionFactoryInvalidProperty, "propertyKey");
        }

        # 使用标准化的属性名称、字符串值和操作符创建搜索条件
        return CreateLeafCondition(canonicalName, value, operation);
    }

    # 创建一个搜索条件，使用属性键、日期时间值和操作符
    public static SearchCondition CreateLeafCondition(PropertyKey propertyKey, DateTime value, SearchConditionOperation operation)
    {
        # 从属性键中获取标准化的属性名称
        PropertySystemNativeMethods.PSGetNameFromPropertyKey(ref propertyKey, out var canonicalName);

        # 检查属性名称是否为空或null，如果是则抛出异常
        if (string.IsNullOrEmpty(canonicalName))
        {
            throw new ArgumentException(LocalizedMessages.SearchConditionFactoryInvalidProperty, "propertyKey");
        }
        # 使用标准化的属性名称、日期时间值和操作符创建搜索条件
        return CreateLeafCondition(canonicalName, value, operation);
    }

    # 创建一个搜索条件，使用属性键、布尔值和操作符
    public static SearchCondition CreateLeafCondition(PropertyKey propertyKey, bool value, SearchConditionOperation operation)
    {
        # 从属性键中获取标准化的属性名称
        PropertySystemNativeMethods.PSGetNameFromPropertyKey(ref propertyKey, out var canonicalName);

        # 检查属性名称是否为空或null，如果是则抛出异常
        if (string.IsNullOrEmpty(canonicalName))
        {
            throw new ArgumentException(LocalizedMessages.SearchConditionFactoryInvalidProperty, "propertyKey");
        }
        # 使用标准化的属性名称、布尔值和操作符创建搜索条件
        return CreateLeafCondition(canonicalName, value, operation);
    }

    # 创建一个搜索条件，使用属性键、双精度浮点值和操作符
    public static SearchCondition CreateLeafCondition(PropertyKey propertyKey, double value, SearchConditionOperation operation)
    {
        # 从属性键中获取标准化的属性名称
        PropertySystemNativeMethods.PSGetNameFromPropertyKey(ref propertyKey, out var canonicalName);

        # 检查属性名称是否为空或null，如果是则抛出异常
        if (string.IsNullOrEmpty(canonicalName))
        {
            throw new ArgumentException(LocalizedMessages.SearchConditionFactoryInvalidProperty, "propertyKey");
        }
        # 使用标准化的属性名称、双精度浮点值和操作符创建搜索条件
        return CreateLeafCondition(canonicalName, value, operation);
    }

    # 创建一个搜索条件，使用属性键、整数值和操作符
    public static SearchCondition CreateLeafCondition(PropertyKey propertyKey, int value, SearchConditionOperation operation)
    {
        # 从属性键中获取标准化的属性名称
        PropertySystemNativeMethods.PSGetNameFromPropertyKey(ref propertyKey, out var canonicalName);

        # 检查属性名称是否为空或null，如果是则抛出异常
        if (string.IsNullOrEmpty(canonicalName))
        {
            throw new ArgumentException(LocalizedMessages.SearchConditionFactoryInvalidProperty, "propertyKey");
        }
        # 使用标准化的属性名称、整数值和操作符创建搜索条件
        return CreateLeafCondition(canonicalName, value, operation);
    }

    # 创建一个取反的搜索条件
    public static SearchCondition CreateNotCondition(SearchCondition conditionToBeNegated, bool simplify)
    {
        # 检查传入的条件是否为空，如果为空则抛出参数为空异常
        if (conditionToBeNegated == null)
        {
            throw new ArgumentNullException("conditionToBeNegated");
        }

        # 创建一个新的条件工厂实例，用于生成条件对象
        var nativeConditionFactory = (IConditionFactory)new ConditionFactoryCoClass();
        ICondition result;

        try
        {
            # 调用条件工厂的 MakeNot 方法，生成 negated 的条件
            var hr = nativeConditionFactory.MakeNot(conditionToBeNegated.NativeSearchCondition, simplify, out result);

            # 检查方法调用是否成功，如果失败则抛出自定义异常
            if (!CoreErrorHelper.Succeeded(hr)) { throw new ShellException(hr); }
        }
        finally
        {
            # 确保即使在出现异常时也释放条件工厂的 COM 对象
            if (nativeConditionFactory != null)
            {
                Marshal.ReleaseComObject(nativeConditionFactory);
            }
        }

        # 返回生成的 SearchCondition 对象
        return new SearchCondition(result);
    }

    # 公共静态方法，解析结构化查询，使用默认文化信息
    public static SearchCondition ParseStructuredQuery(string query) => ParseStructuredQuery(query, null!);

    # 公共静态方法，解析结构化查询，允许指定文化信息
    public static SearchCondition ParseStructuredQuery(string query, CultureInfo cultureInfo)
    {
        # 如果查询字符串为空，则抛出参数为空异常
        if (string.IsNullOrEmpty(query))
        {
            throw new ArgumentNullException("query");
        }

        # 创建一个新的 QueryParserManager 实例，并将其转换为 IQueryParserManager 类型
        var nativeQueryParserManager = (IQueryParserManager)new QueryParserManagerCoClass();
        # 初始化 IQueryParser 和 IQuerySolution 对象
        IQueryParser queryParser = null!;
        IQuerySolution querySolution = null!;
        ICondition result = null!;

        # 初始化 IEntity 和 SearchCondition 对象
        IEntity mainType = null!;
        SearchCondition searchCondition = null!;
        try
        {
            # 创建一个 Guid 对象，标识 IQueryParser 接口
            var guid = new Guid(ShellIIDGuid.IQueryParser);
            # 调用 CreateLoadedParser 方法，创建一个新的解析器
            var hr = nativeQueryParserManager.CreateLoadedParser(
                "SystemIndex",
                cultureInfo == null ? (ushort)0 : (ushort)cultureInfo.LCID,
                ref guid,
                out queryParser);

            # 检查方法调用是否成功，如果失败则抛出 ShellException 异常
            if (!CoreErrorHelper.Succeeded(hr)) { throw new ShellException(hr); }

            # 如果 queryParser 不为空，则设置解析器选项
            if (queryParser != null)
            {
                using (var optionValue = new PropVariant(true))
                {
                    hr = queryParser.SetOption(StructuredQuerySingleOption.NaturalSyntax, optionValue);
                }

                # 检查设置选项是否成功，如果失败则抛出 ShellException 异常
                if (!CoreErrorHelper.Succeeded(hr)) { throw new ShellException(hr); }

                # 解析查询字符串，并获取查询解决方案
                hr = queryParser.Parse(query, null!, out querySolution);

                # 检查解析是否成功，如果失败则抛出 ShellException 异常
                if (!CoreErrorHelper.Succeeded(hr)) { throw new ShellException(hr); }

                # 如果 querySolution 不为空，则获取最终的查询条件
                if (querySolution != null)
                {
                    hr = querySolution.GetQuery(out result, out mainType);

                    # 检查获取查询条件是否成功，如果失败则抛出 ShellException 异常
                    if (!CoreErrorHelper.Succeeded(hr)) { throw new ShellException(hr); }
                }
            }

            # 使用获取的查询条件创建 SearchCondition 对象，并返回
            searchCondition = new SearchCondition(result);
            return searchCondition;
        }
        catch
        {
            # 在发生异常时，释放 SearchCondition 对象
            if (searchCondition != null) { searchCondition.Dispose(); }
            throw;
        }
        finally
        {
            # 释放 COM 对象，以防止内存泄漏
            if (nativeQueryParserManager != null)
            {
                Marshal.ReleaseComObject(nativeQueryParserManager);
            }

            if (queryParser != null)
            {
                Marshal.ReleaseComObject(queryParser);
            }

            if (querySolution != null)
            {
                Marshal.ReleaseComObject(querySolution);
            }

            if (mainType != null)
            {
                Marshal.ReleaseComObject(mainType);
            }
        }
    }

    # Suppress CA2000 警告：在失去作用域之前处理对象，确保资源得到释放
    [System.Diagnostics.CodeAnalysis.SuppressMessage("Microsoft.Reliability", "CA2000:Dispose objects before losing scope")]
    private static SearchCondition CreateLeafCondition(string propertyName, PropVariant propVar, string valueType, SearchConditionOperation operation)
    {
        # 初始化本地条件工厂为 null
        IConditionFactory nativeConditionFactory = null!;
        # 初始化搜索条件为 null
        SearchCondition condition = null!;

        try
        {
            # 创建条件工厂对象并强制转换为 IConditionFactory
            nativeConditionFactory = (IConditionFactory)new ConditionFactoryCoClass();

            # 如果属性名称为空或为 "SYSTEM.NULL"，则将属性名称设置为 null
            if (string.IsNullOrEmpty(propertyName) || propertyName.ToUpperInvariant() == "SYSTEM.NULL")
            {
                propertyName = null!;
            }

            # 初始化结果代码为失败状态
            var hr = HResult.Fail;

            # 调用条件工厂的 MakeLeaf 方法创建叶条件
            hr = nativeConditionFactory.MakeLeaf(propertyName, operation, valueType,
                propVar, null!, null!, null!, false, out var nativeCondition);

            # 如果 MakeLeaf 方法返回失败，抛出 ShellException 异常
            if (!CoreErrorHelper.Succeeded(hr))
            {
                throw new ShellException(hr);
            }

            # 根据返回的本地条件创建搜索条件对象
            condition = new SearchCondition(nativeCondition);
        }
        finally
        {
            # 如果条件工厂对象不为 null，则释放其 COM 对象
            if (nativeConditionFactory != null)
            {
                Marshal.ReleaseComObject(nativeConditionFactory);
            }
        }

        # 返回创建的搜索条件对象
        return condition;
    }
你给的代码似乎不完整，能否提供更多上下文或完整代码？这样我可以更准确地为每个语句添加注释。
```