# `.\better-genshin-impact\Build\MicaSetup\Natives\Shell\Dialogs\PropertySystem\ShellPropertyFactory.cs`

```cs
﻿using System;
using System.Collections.Generic;
using System.Diagnostics.CodeAnalysis;
using System.Linq;
using System.Linq.Expressions;
using System.Reflection;
using System.Runtime.InteropServices;
using System.Runtime.InteropServices.ComTypes;

namespace MicaSetup.Shell.Dialogs;

internal static class ShellPropertyFactory
{
    // 存储创建 IShellProperty 对象的缓存，键为 PropertyKey，值为构造函数
    private static readonly Dictionary<int, Func<PropertyKey, ShellPropertyDescription, object, IShellProperty>> _storeCache = new();

    // 根据 PropertyKey 和 ShellObject 创建 IShellProperty 对象
    public static IShellProperty CreateShellProperty(PropertyKey propKey, ShellObject shellObject) => GenericCreateShellProperty(propKey, shellObject);

    // 根据 PropertyKey 和 IPropertyStore 创建 IShellProperty 对象
    public static IShellProperty CreateShellProperty(PropertyKey propKey, IPropertyStore store) => GenericCreateShellProperty(propKey, store);

    // 根据 VarEnum 类型转换为对应的 .NET 类型
    [SuppressMessage("Microsoft.Maintainability", "CA1502:")]
    public static Type VarEnumToSystemType(VarEnum VarEnumType)
    {
        return VarEnumType switch
        {
            // 对应 VarEnum.VT_EMPTY 或 VarEnum.VT_NULL 类型，返回 object 类型
            (VarEnum.VT_EMPTY) or (VarEnum.VT_NULL) => typeof(object),
            // 对应 VarEnum.VT_UI1 类型，返回 byte? 类型
            (VarEnum.VT_UI1) => typeof(byte?),
            // 对应 VarEnum.VT_I2 类型，返回 short? 类型
            (VarEnum.VT_I2) => typeof(short?),
            // 对应 VarEnum.VT_UI2 类型，返回 ushort? 类型
            (VarEnum.VT_UI2) => typeof(ushort?),
            // 对应 VarEnum.VT_I4 类型，返回 int? 类型
            (VarEnum.VT_I4) => typeof(int?),
            // 对应 VarEnum.VT_UI4 类型，返回 uint? 类型
            (VarEnum.VT_UI4) => typeof(uint?),
            // 对应 VarEnum.VT_I8 类型，返回 long? 类型
            (VarEnum.VT_I8) => typeof(long?),
            // 对应 VarEnum.VT_UI8 类型，返回 ulong? 类型
            (VarEnum.VT_UI8) => typeof(ulong?),
            // 对应 VarEnum.VT_R8 类型，返回 double? 类型
            (VarEnum.VT_R8) => typeof(double?),
            // 对应 VarEnum.VT_BOOL 类型，返回 bool? 类型
            (VarEnum.VT_BOOL) => typeof(bool?),
            // 对应 VarEnum.VT_FILETIME 类型，返回 DateTime? 类型
            (VarEnum.VT_FILETIME) => typeof(DateTime?),
            // 对应 VarEnum.VT_CLSID 类型，返回 IntPtr? 类型
            (VarEnum.VT_CLSID) => typeof(IntPtr?),
            // 对应 VarEnum.VT_CF 类型，返回 IntPtr? 类型
            (VarEnum.VT_CF) => typeof(IntPtr?),
            // 对应 VarEnum.VT_BLOB 类型，返回 byte[] 类型
            (VarEnum.VT_BLOB) => typeof(byte[]),
            // 对应 VarEnum.VT_LPWSTR 类型，返回 string 类型
            (VarEnum.VT_LPWSTR) => typeof(string),
            // 对应 VarEnum.VT_UNKNOWN 类型，返回 IntPtr? 类型
            (VarEnum.VT_UNKNOWN) => typeof(IntPtr?),
            // 对应 VarEnum.VT_STREAM 类型，返回 IStream 类型
            (VarEnum.VT_STREAM) => typeof(IStream),
            // 对应 VarEnum.VT_VECTOR | VarEnum.VT_UI1 类型，返回 byte[] 类型
            (VarEnum.VT_VECTOR | VarEnum.VT_UI1) => typeof(byte[]),
            // 对应 VarEnum.VT_VECTOR | VarEnum.VT_I2 类型，返回 short[] 类型
            (VarEnum.VT_VECTOR | VarEnum.VT_I2) => typeof(short[]),
            // 对应 VarEnum.VT_VECTOR | VarEnum.VT_UI2 类型，返回 ushort[] 类型
            (VarEnum.VT_VECTOR | VarEnum.VT_UI2) => typeof(ushort[]),
            // 对应 VarEnum.VT_VECTOR | VarEnum.VT_I4 类型，返回 int[] 类型
            (VarEnum.VT_VECTOR | VarEnum.VT_I4) => typeof(int[]),
            // 对应 VarEnum.VT_VECTOR | VarEnum.VT_UI4 类型，返回 uint[] 类型
            (VarEnum.VT_VECTOR | VarEnum.VT_UI4) => typeof(uint[]),
            // 对应 VarEnum.VT_VECTOR | VarEnum.VT_I8 类型，返回 long[] 类型
            (VarEnum.VT_VECTOR | VarEnum.VT_I8) => typeof(long[]),
            // 对应 VarEnum.VT_VECTOR | VarEnum.VT_UI8 类型，返回 ulong[] 类型
            (VarEnum.VT_VECTOR | VarEnum.VT_UI8) => typeof(ulong[]),
            // 对应 VarEnum.VT_VECTOR | VarEnum.VT_R8 类型，返回 double[] 类型
            (VarEnum.VT_VECTOR | VarEnum.VT_R8) => typeof(double[]),
            // 对应 VarEnum.VT_VECTOR | VarEnum.VT_BOOL 类型，返回 bool[] 类型
            (VarEnum.VT_VECTOR | VarEnum.VT_BOOL) => typeof(bool[]),
            // 对应 VarEnum.VT_VECTOR | VarEnum.VT_FILETIME 类型，返回 DateTime[] 类型
            (VarEnum.VT_VECTOR | VarEnum.VT_FILETIME) => typeof(DateTime[]),
            // 对应 VarEnum.VT_VECTOR | VarEnum.VT_CLSID 类型，返回 IntPtr[] 类型
            (VarEnum.VT_VECTOR | VarEnum.VT_CLSID) => typeof(IntPtr[]),
            // 对应 VarEnum.VT_VECTOR | VarEnum.VT_CF 类型，返回 IntPtr[] 类型
            (VarEnum.VT_VECTOR | VarEnum.VT_CF) => typeof(IntPtr[]),
            // 对应 VarEnum.VT_VECTOR | VarEnum.VT_LPWSTR 类型，返回 string[] 类型
            (VarEnum.VT_VECTOR | VarEnum.VT_LPWSTR) => typeof(string[]),
            // 默认返回 object 类型
            _ => typeof(object),
        };
    }

    // 声明一个方法，接收类型和参数类型数组，返回构造函数
    private static Func<PropertyKey, ShellPropertyDescription, object, IShellProperty> ExpressConstructor(Type type, Type[] argTypes)
    {
        # 获取参数类型的哈希值
        var typeHash = GetTypeHash(argTypes);

        # 查找匹配的构造函数信息，要求构造函数的参数类型哈希值与 typeHash 相同
        var ctorInfo = type.GetConstructors(BindingFlags.Instance | BindingFlags.NonPublic | BindingFlags.Public)
            .FirstOrDefault(x => typeHash == GetTypeHash(x.GetParameters().Select(a => a.ParameterType)));

        # 如果没有找到匹配的构造函数，则抛出异常
        if (ctorInfo == null)
        {
            throw new ArgumentException(LocalizedMessages.ShellPropertyFactoryConstructorNotFound, "type");
        }

        # 定义表达式参数，用于构造 Lambda 表达式
        var key = Expression.Parameter(argTypes[0], "propKey");
        var desc = Expression.Parameter(argTypes[1], "desc");
        var third = Expression.Parameter(typeof(object), "third");

        # 创建一个调用构造函数的表达式
        var create = Expression.New(ctorInfo, key, desc,
            Expression.Convert(third, argTypes[2]));

        # 返回一个编译好的 Lambda 表达式，用于创建 IShellProperty 实例
        return Expression.Lambda<Func<PropertyKey, ShellPropertyDescription, object, IShellProperty>>(
            create, key, desc, third).Compile();
    }

    # 创建一个泛型的 IShellProperty 实例
    private static IShellProperty GenericCreateShellProperty<T>(PropertyKey propKey, T thirdArg)
    {
        # 判断 thirdArg 的实际类型，如果是 ShellObject 则使用 ShellObject 类型，否则使用 T 类型
        var thirdType = (thirdArg is ShellObject) ? typeof(ShellObject) : typeof(T);

        # 从缓存中获取属性描述
        var propDesc = ShellPropertyDescriptionsCache.Cache.GetPropertyDescription(propKey);

        # 创建泛型类型的 ShellProperty 类型
        var type = typeof(ShellProperty<>).MakeGenericType(VarEnumToSystemType(propDesc.VarEnumType));

        # 获取类型哈希值，用于查找构造函数缓存
        var hash = GetTypeHash(type, thirdType);

        # 如果缓存中没有构造函数，则创建并缓存构造函数
        if (!_storeCache.TryGetValue(hash, out var ctor))
        {
            Type[] argTypes = { typeof(PropertyKey), typeof(ShellPropertyDescription), thirdType };
            ctor = ExpressConstructor(type, argTypes);
            _storeCache.Add(hash, ctor);
        }

        # 使用构造函数创建 IShellProperty 实例
        return ctor(propKey, propDesc, thirdArg!);
    }

    # 计算类型的哈希值（接收多个类型参数）
    private static int GetTypeHash(params Type[] types) => GetTypeHash((IEnumerable<Type>)types);

    # 计算类型的哈希值（接收类型集合）
    private static int GetTypeHash(IEnumerable<Type> types)
    {
        var hash = 0;
        # 对每个类型计算哈希值
        foreach (var type in types)
        {
            hash = hash * 31 + type.GetHashCode();
        }
        # 返回计算后的哈希值
        return hash;
    }
你提供的代码片段似乎不完整。可以请你确认一下吗？
```