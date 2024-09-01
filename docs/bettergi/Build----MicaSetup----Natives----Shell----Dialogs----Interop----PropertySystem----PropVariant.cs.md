# `.\better-genshin-impact\Build\MicaSetup\Natives\Shell\Dialogs\Interop\PropertySystem\PropVariant.cs`

```cs
﻿using System; // 引入 System 命名空间，包含基本类型和功能
using System.Collections.Generic; // 引入 System.Collections.Generic 命名空间，包含集合类
using System.Linq.Expressions; // 引入 System.Linq.Expressions 命名空间，包含表达式树
using System.Runtime.InteropServices; // 引入 System.Runtime.InteropServices 命名空间，提供与非托管代码的互操作功能

namespace MicaSetup.Shell.Dialogs; // 定义 MicaSetup.Shell.Dialogs 命名空间

[StructLayout(LayoutKind.Explicit)] // 使用显式布局，定义字段在内存中的位置
public sealed class PropVariant : IDisposable // 定义一个封闭类 PropVariant 实现 IDisposable 接口
{
    [StructLayout(LayoutKind.Sequential)] // 使用顺序布局，定义字段在内存中的位置
    private struct Blob // 定义一个结构体 Blob
    {
        public int Number; // 整数字段 Number
        public nint Pointer; // 指针字段 Pointer
    }

    private static Dictionary<Type, Action<PropVariant, Array, uint>> _vectorActions = null!; // 静态字典，用于存储类型到操作的映射，初始值为 null

    private static Dictionary<Type, Action<PropVariant, Array, uint>> GenerateVectorActions() // 生成用于处理向量的操作的方法
    }

    public static PropVariant FromObject(object value) // 静态方法，根据对象类型创建 PropVariant 实例
    {
        if (value == null) // 如果输入值为 null
        {
            return new PropVariant(); // 返回一个新的 PropVariant 实例
        }
        else // 如果输入值不为 null
        {
            Func<object, PropVariant> func = GetDynamicConstructor(value.GetType()); // 获取动态构造函数
            return func(value); // 使用构造函数创建 PropVariant 实例并返回
        }
    }

    private static readonly Dictionary<Type, Func<object, PropVariant>> _cache = new(); // 静态字典，用于缓存类型到构造函数的映射

    private static readonly object _padlock = new(); // 静态对象，用于在多线程环境下锁定

    private static Func<object, PropVariant> GetDynamicConstructor(Type type) // 获取用于指定类型的动态构造函数
    {
        lock (_padlock) // 锁定以确保线程安全
        {
            if (!_cache.TryGetValue(type, out var action)) // 尝试从缓存中获取构造函数
            {
                System.Reflection.ConstructorInfo constructor = typeof(PropVariant) // 获取 PropVariant 的构造函数
                    .GetConstructor(new Type[] { type });

                if (constructor == null) // 如果没有找到合适的构造函数
                {
                    throw new ArgumentException(LocalizedMessages.PropVariantTypeNotSupported); // 抛出参数异常
                }
                else // 如果找到了构造函数
                {
                    ParameterExpression arg = Expression.Parameter(typeof(object), "arg"); // 创建表达式参数
                    NewExpression create = Expression.New(constructor, Expression.Convert(arg, type)); // 创建新实例的表达式

                    action = Expression.Lambda<Func<object, PropVariant>>(create, arg).Compile(); // 创建 Lambda 表达式并编译
                    _cache.Add(type, action); // 将构造函数添加到缓存
                }
            }
            return action; // 返回构造函数
        }
    }

    [FieldOffset(0)] // 指定字段在内存中的偏移量为 0
    private readonly decimal _decimal; // 只读字段 _decimal

    [FieldOffset(0)] // 指定字段在内存中的偏移量为 0
    private ushort _valueType; // 字段 _valueType，存储值的类型

    [FieldOffset(8)] // 指定字段在内存中的偏移量为 8
    private readonly Blob _blob; // 只读字段 _blob，存储 Blob 结构

    [FieldOffset(8)] // 指定字段在内存中的偏移量为 8
    private nint _ptr; // 字段 _ptr，存储指针

    [FieldOffset(8)] // 指定字段在内存中的偏移量为 8
    private readonly int _int32; // 只读字段 _int32，存储 32 位整数

    [FieldOffset(8)] // 指定字段在内存中的偏移量为 8
    private readonly uint _uint32; // 只读字段 _uint32，存储 32 位无符号整数

    [FieldOffset(8)] // 指定字段在内存中的偏移量为 8
    private readonly byte _byte; // 只读字段 _byte，存储字节

    [FieldOffset(8)] // 指定字段在内存中的偏移量为 8
    private readonly sbyte _sbyte; // 只读字段 _sbyte，存储带符号字节

    [FieldOffset(8)] // 指定字段在内存中的偏移量为 8
    private readonly short _short; // 只读字段 _short，存储 16 位整数

    [FieldOffset(8)] // 指定字段在内存中的偏移量为 8
    private readonly ushort _ushort; // 只读字段 _ushort，存储 16 位无符号整数

    [FieldOffset(8)] // 指定字段在内存中的偏移量为 8
    private readonly long _long; // 只读字段 _long，存储 64 位整数

    [FieldOffset(8)] // 指定字段在内存中的偏移量为 8
    private readonly ulong _ulong; // 只读字段 _ulong，存储 64 位无符号整数

    [FieldOffset(8)] // 指定字段在内存中的偏移量为 8
    private readonly double _double; // 只读字段 _double，存储双精度浮点数

    [FieldOffset(8)] // 指定字段在内存中的偏移量为 8
    private readonly float _float; // 只读字段 _float，存储单精度浮点数

    public PropVariant() // 默认构造函数
    {
    }

    public PropVariant(string value) // 以字符串值为参数的构造函数
    {
        # 检查传入的值是否为 null
        if (value == null)
        {
            # 如果值为 null，抛出一个带有本地化消息的 ArgumentException 异常
            throw new ArgumentException(LocalizedMessages.PropVariantNullString, "value");
        }

        # 设置 _valueType 字段为 VT_LPWSTR (宽字符字符串)
        _valueType = (ushort)VarEnum.VT_LPWSTR;
        # 将字符串转换为 Unicode 字符串并分配到 _ptr
        _ptr = Marshal.StringToCoTaskMemUni(value);
    }

    # 带有布尔值的构造函数
    public PropVariant(bool value)
    {
        # 设置 _valueType 字段为 VT_BOOL (布尔值)
        _valueType = (ushort)VarEnum.VT_BOOL;
        # 将布尔值转换为 -1 (true) 或 0 (false)，并赋值给 _int32
        _int32 = (value == true) ? -1 : 0;
    }

    # 带有 DateTime 值的构造函数
    public PropVariant(DateTime value)
    {
        # 设置 _valueType 字段为 VT_FILETIME (文件时间)
        _valueType = (ushort)VarEnum.VT_FILETIME;

        # 将 DateTime 转换为 FILETIME 结构
        System.Runtime.InteropServices.ComTypes.FILETIME ft = DateTimeToFileTime(value);
        # 使用 FILETIME 结构初始化 PropVariant 对象
        PropVariantNativeMethods.InitPropVariantFromFileTime(ref ft, this);
    }

    # 带有整数值的构造函数
    public PropVariant(int value)
    {
        # 设置 _valueType 字段为 VT_I4 (32 位整数)
        _valueType = (ushort)VarEnum.VT_I4;
        # 将整数值赋值给 _int32
        _int32 = value;
    }

    # 带有双精度浮点数值的构造函数
    public PropVariant(double value)
    {
        # 设置 _valueType 字段为 VT_R8 (双精度浮点数)
        _valueType = (ushort)VarEnum.VT_R8;
        # 将浮点数值赋值给 _double
        _double = value;
    }

    # 属性，用于获取或设置值类型
    public VarEnum VarType
    {
        # 获取 _valueType 字段并转换为 VarEnum 类型
        get => (VarEnum)_valueType;
        # 设置 _valueType 字段为指定的 VarEnum 值
        set => _valueType = (ushort)value;
    }

    # 属性，用于检查是否为 null 或空
    public bool IsNullOrEmpty => (_valueType == (ushort)VarEnum.VT_EMPTY || _valueType == (ushort)VarEnum.VT_NULL);

    # 属性，用于获取或设置值
    public object Value
    {
        get
        {
            // 根据 _valueType 的值，从不同的字段中返回相应的数据
            return (VarEnum)_valueType switch
            {
                // 如果 _valueType 是 VT_I1，则返回 _sbyte 字段
                VarEnum.VT_I1 => _sbyte,
                // 如果 _valueType 是 VT_UI1，则返回 _byte 字段
                VarEnum.VT_UI1 => _byte,
                // 如果 _valueType 是 VT_I2，则返回 _short 字段
                VarEnum.VT_I2 => _short,
                // 如果 _valueType 是 VT_UI2，则返回 _ushort 字段
                VarEnum.VT_UI2 => _ushort,
                // 如果 _valueType 是 VT_I4 或 VT_INT，则返回 _int32 字段
                VarEnum.VT_I4 or VarEnum.VT_INT => _int32,
                // 如果 _valueType 是 VT_UI4 或 VT_UINT，则返回 _uint32 字段
                VarEnum.VT_UI4 or VarEnum.VT_UINT => _uint32,
                // 如果 _valueType 是 VT_I8，则返回 _long 字段
                VarEnum.VT_I8 => _long,
                // 如果 _valueType 是 VT_UI8，则返回 _ulong 字段
                VarEnum.VT_UI8 => _ulong,
                // 如果 _valueType 是 VT_R4，则返回 _float 字段
                VarEnum.VT_R4 => _float,
                // 如果 _valueType 是 VT_R8，则返回 _double 字段
                VarEnum.VT_R8 => _double,
                // 如果 _valueType 是 VT_BOOL，则返回 _int32 是否等于 -1
                VarEnum.VT_BOOL => _int32 == -1,
                // 如果 _valueType 是 VT_ERROR，则返回 _long 字段
                VarEnum.VT_ERROR => _long,
                // 如果 _valueType 是 VT_CY，则返回 _decimal 字段
                VarEnum.VT_CY => _decimal,
                // 如果 _valueType 是 VT_DATE，则将 _double 转换为 DateTime 对象
                VarEnum.VT_DATE => DateTime.FromOADate(_double),
                // 如果 _valueType 是 VT_FILETIME，则将 _long 转换为 DateTime 对象
                VarEnum.VT_FILETIME => DateTime.FromFileTime(_long),
                // 如果 _valueType 是 VT_BSTR，则将 _ptr 指针转换为 BSTR 字符串
                VarEnum.VT_BSTR => Marshal.PtrToStringBSTR(_ptr),
                // 如果 _valueType 是 VT_BLOB，则调用 GetBlobData 方法获取 BLOB 数据
                VarEnum.VT_BLOB => GetBlobData(),
                // 如果 _valueType 是 VT_LPSTR，则将 _ptr 指针转换为 ANSI 字符串
                VarEnum.VT_LPSTR => Marshal.PtrToStringAnsi(_ptr),
                // 如果 _valueType 是 VT_LPWSTR，则将 _ptr 指针转换为 Unicode 字符串
                VarEnum.VT_LPWSTR => Marshal.PtrToStringUni(_ptr),
                // 如果 _valueType 是 VT_UNKNOWN，则将 _ptr 指针转换为 IUnknown 对象
                VarEnum.VT_UNKNOWN => Marshal.GetObjectForIUnknown(_ptr),
                // 如果 _valueType 是 VT_DISPATCH，则将 _ptr 指针转换为 IDispatch 对象
                VarEnum.VT_DISPATCH => Marshal.GetObjectForIUnknown(_ptr),
                // 如果 _valueType 是 VT_DECIMAL，则返回 _decimal 字段
                VarEnum.VT_DECIMAL => _decimal,
                // 如果 _valueType 是 VT_ARRAY 或 VT_UNKNOWN，则调用 CrackSingleDimSafeArray 方法处理单维安全数组
                VarEnum.VT_ARRAY | VarEnum.VT_UNKNOWN => CrackSingleDimSafeArray(_ptr),
                // 如果 _valueType 是 VT_VECTOR | VT_LPWSTR，则调用 GetVector 方法获取字符串向量
                (VarEnum.VT_VECTOR | VarEnum.VT_LPWSTR) => GetVector<string>(),
                // 如果 _valueType 是 VT_VECTOR | VT_I2，则调用 GetVector 方法获取 short 类型的向量
                (VarEnum.VT_VECTOR | VarEnum.VT_I2) => GetVector<short>(),
                // 如果 _valueType 是 VT_VECTOR | VT_UI2，则调用 GetVector 方法获取 ushort 类型的向量
                (VarEnum.VT_VECTOR | VarEnum.VT_UI2) => GetVector<ushort>(),
                // 如果 _valueType 是 VT_VECTOR | VT_I4，则调用 GetVector 方法获取 int 类型的向量
                (VarEnum.VT_VECTOR | VarEnum.VT_I4) => GetVector<int>(),
                // 如果 _valueType 是 VT_VECTOR | VT_UI4，则调用 GetVector 方法获取 uint 类型的向量
                (VarEnum.VT_VECTOR | VarEnum.VT_UI4) => GetVector<uint>(),
                // 如果 _valueType 是 VT_VECTOR | VT_I8，则调用 GetVector 方法获取 long 类型的向量
                (VarEnum.VT_VECTOR | VarEnum.VT_I8) => GetVector<long>(),
                // 如果 _valueType 是 VT_VECTOR | VT_UI8，则调用 GetVector 方法获取 ulong 类型的向量
                (VarEnum.VT_VECTOR | VarEnum.VT_UI8) => GetVector<ulong>(),
                // 如果 _valueType 是 VT_VECTOR | VT_R4，则调用 GetVector 方法获取 float 类型的向量
                (VarEnum.VT_VECTOR | VarEnum.VT_R4) => GetVector<float>(),
                // 如果 _valueType 是 VT_VECTOR | VT_R8，则调用 GetVector 方法获取 double 类型的向量
                (VarEnum.VT_VECTOR | VarEnum.VT_R8) => GetVector<double>(),
                // 如果 _valueType 是 VT_VECTOR | VT_BOOL，则调用 GetVector 方法获取 bool 类型的向量
                (VarEnum.VT_VECTOR | VarEnum.VT_BOOL) => GetVector<bool>(),
                // 如果 _valueType 是 VT_VECTOR | VT_FILETIME，则调用 GetVector 方法获取 DateTime 类型的向量
                (VarEnum.VT_VECTOR | VarEnum.VT_FILETIME) => GetVector<DateTime>(),
                // 如果 _valueType 是 VT_VECTOR | VT_DECIMAL，则调用 GetVector 方法获取 decimal 类型的向量
                (VarEnum.VT_VECTOR | VarEnum.VT_DECIMAL) => GetVector<decimal>(),
                // 如果 _valueType 不匹配任何已知的值，则返回 null
                _ => null!,
            };
        }
    }

    // 将 FILETIME 结构体的值转换为 long 类型
    private static long GetFileTimeAsLong(ref System.Runtime.InteropServices.ComTypes.FILETIME val) => (((long)val.dwHighDateTime) << 32) + val.dwLowDateTime;

    // 将 DateTime 对象转换为 FILETIME 结构体
    private static System.Runtime.InteropServices.ComTypes.FILETIME DateTimeToFileTime(DateTime value)
    {
        // 将 DateTime 转换为文件时间的 long 值
        long hFT = value.ToFileTime();
        // 创建 FILETIME 结构体并填充低位和高位的日期时间
        System.Runtime.InteropServices.ComTypes.FILETIME ft =
            new()
            {
                dwLowDateTime = (int)(hFT & 0xFFFFFFFF),
                dwHighDateTime = (int)(hFT >> 32)
            };
        return ft;
    }

    // 获取 BLOB 数据
    private object GetBlobData()
    # 创建一个指定大小的字节数组
    {
        byte[] blobData = new byte[_int32];

        # 获取 _blob 指针的值
        nint pBlobData = _blob.Pointer;
        # 从指针位置复制数据到 blobData 数组中
        Marshal.Copy(pBlobData, blobData, 0, _int32);

        # 返回字节数组
        return blobData;
    }

    # 获取指定类型的向量数组
    private Array GetVector<T>()
    {
        # 获取元素数量
        int count = PropVariantNativeMethods.PropVariantGetElementCount(this);
        # 如果元素数量小于或等于0，返回空数组
        if (count <= 0) { return null!; }

        # 线程安全地初始化 _vectorActions
        lock (_padlock)
        {
            _vectorActions ??= GenerateVectorActions();
        }

        # 尝试从 _vectorActions 中获取指定类型的动作
        if (!_vectorActions.TryGetValue(typeof(T), out var action))
        {
            # 如果未找到指定类型的动作，抛出异常
            throw new InvalidCastException(LocalizedMessages.PropVariantUnsupportedType);
        }

        # 创建指定类型的数组
        Array array = new T[count];
        # 根据动作填充数组
        for (uint i = 0; i < count; i++)
        {
            action(this, array, i);
        }

        # 返回填充好的数组
        return array;
    }

    # 解析一维安全数组
    private static Array CrackSingleDimSafeArray(nint psa)
    {
        # 获取数组维度
        uint cDims = PropVariantNativeMethods.SafeArrayGetDim(psa);
        # 如果不是一维数组，抛出异常
        if (cDims != 1)
            throw new ArgumentException(LocalizedMessages.PropVariantMultiDimArray, "psa");

        # 获取数组的下界
        int lBound = PropVariantNativeMethods.SafeArrayGetLBound(psa, 1U);
        # 获取数组的上界
        int uBound = PropVariantNativeMethods.SafeArrayGetUBound(psa, 1U);

        # 计算数组的长度
        int n = uBound - lBound + 1;

        # 创建对象数组
        object[] array = new object[n];
        # 从安全数组中提取元素
        for (int i = lBound; i <= uBound; ++i)
        {
            array[i] = PropVariantNativeMethods.SafeArrayGetElement(psa, ref i);
        }

        # 返回对象数组
        return array;
    }

    # 释放资源
    public void Dispose()
    {
        # 清除 PropVariant 对象
        PropVariantNativeMethods.PropVariantClear(this);

        # 防止垃圾回收器调用析构函数
        GC.SuppressFinalize(this);
    }

    # 析构函数
    ~PropVariant()
    {
        # 调用 Dispose 方法释放资源
        Dispose();
    }

    # 返回对象的字符串表示形式
    public override string ToString() => string.Format(System.Globalization.CultureInfo.InvariantCulture,
            "{0}: {1}", Value, VarType.ToString());
# 结束一个代码块或语句的标志（可能是函数、条件语句或循环体）
}
```