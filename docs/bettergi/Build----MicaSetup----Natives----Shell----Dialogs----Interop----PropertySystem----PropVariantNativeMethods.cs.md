# `.\better-genshin-impact\Build\MicaSetup\Natives\Shell\Dialogs\Interop\PropertySystem\PropVariantNativeMethods.cs`

```cs
# 声明用于调用原生方法的类
namespace MicaSetup.Shell.Dialogs;

# 定义用于与 PropVariant 相关的原生方法的静态类
internal static class PropVariantNativeMethods
{
    # 从布尔值数组初始化 PropVariant
    [DllImport(Lib.PropSys, CharSet = CharSet.Unicode, SetLastError = true, PreserveSig = false)]
    internal static extern void InitPropVariantFromBooleanVector([In, MarshalAs(UnmanagedType.LPArray)] bool[] prgf, uint cElems, [Out] PropVariant ppropvar);

    # 从双精度浮点数数组初始化 PropVariant
    [DllImport(Lib.PropSys, CharSet = CharSet.Unicode, SetLastError = true, PreserveSig = false)]
    internal static extern void InitPropVariantFromDoubleVector([In, Out] double[] prgn, uint cElems, [Out] PropVariant propvar);

    # 从 FILETIME 结构初始化 PropVariant
    [DllImport(Lib.PropSys, CharSet = CharSet.Unicode, SetLastError = true, PreserveSig = false)]
    internal static extern void InitPropVariantFromFileTime([In] ref System.Runtime.InteropServices.ComTypes.FILETIME pftIn, [Out] PropVariant ppropvar);

    # 从 FILETIME 数组初始化 PropVariant
    [DllImport(Lib.PropSys, CharSet = CharSet.Unicode, SetLastError = true, PreserveSig = false)]
    internal static extern void InitPropVariantFromFileTimeVector([In, Out] System.Runtime.InteropServices.ComTypes.FILETIME[] prgft, uint cElems, [Out] PropVariant ppropvar);

    # 从 16 位整数数组初始化 PropVariant
    [DllImport(Lib.PropSys, CharSet = CharSet.Unicode, SetLastError = true, PreserveSig = false)]
    internal static extern void InitPropVariantFromInt16Vector([In, Out] short[] prgn, uint cElems, [Out] PropVariant ppropvar);

    # 从 32 位整数数组初始化 PropVariant
    [DllImport(Lib.PropSys, CharSet = CharSet.Unicode, SetLastError = true, PreserveSig = false)]
    internal static extern void InitPropVariantFromInt32Vector([In, Out] int[] prgn, uint cElems, [Out] PropVariant propVar);

    # 从 64 位整数数组初始化 PropVariant
    [DllImport(Lib.PropSys, CharSet = CharSet.Unicode, SetLastError = true, PreserveSig = false)]
    internal static extern void InitPropVariantFromInt64Vector([In, Out] long[] prgn, uint cElems, [Out] PropVariant ppropvar);

    # 从 PropVariant 的单个元素初始化另一个 PropVariant
    [DllImport(Lib.PropSys, CharSet = CharSet.Unicode, SetLastError = true, PreserveSig = false)]
    internal static extern void InitPropVariantFromPropVariantVectorElem([In] PropVariant propvarIn, uint iElem, [Out] PropVariant ppropvar);

    # 从字符串数组初始化 PropVariant
    [DllImport(Lib.PropSys, CharSet = CharSet.Unicode, SetLastError = true, PreserveSig = false)]
    internal static extern void InitPropVariantFromStringVector([In, Out] string[] prgsz, uint cElems, [Out] PropVariant ppropvar);

    # 从 16 位无符号整数数组初始化 PropVariant
    [DllImport(Lib.PropSys, CharSet = CharSet.Unicode, SetLastError = true, PreserveSig = false)]
    internal static extern void InitPropVariantFromUInt16Vector([In, Out] ushort[] prgn, uint cElems, [Out] PropVariant ppropvar);

    # 从 32 位无符号整数数组初始化 PropVariant
    [DllImport(Lib.PropSys, CharSet = CharSet.Unicode, SetLastError = true, PreserveSig = false)]
    internal static extern void InitPropVariantFromUInt32Vector([In, Out] uint[] prgn, uint cElems, [Out] PropVariant ppropvar);

    # 继续声明其他原生方法
    # 从无符号 64 位整型数组初始化属性变体
    internal static extern void InitPropVariantFromUInt64Vector([In, Out] ulong[] prgn, uint cElems, [Out] PropVariant ppropvar);

    # 清除属性变体的内容
    [DllImport(Lib.Ole32, PreserveSig = false)]
    internal static extern void PropVariantClear([In, Out] PropVariant pvar);

    # 获取属性变体中布尔值元素
    [DllImport(Lib.PropSys, CharSet = CharSet.Unicode, SetLastError = true, PreserveSig = false)]
    internal static extern void PropVariantGetBooleanElem([In] PropVariant propVar, [In] uint iElem, [Out, MarshalAs(UnmanagedType.Bool)] out bool pfVal);

    # 获取属性变体中双精度浮点数元素
    [DllImport(Lib.PropSys, CharSet = CharSet.Unicode, SetLastError = true, PreserveSig = false)]
    internal static extern void PropVariantGetDoubleElem([In] PropVariant propVar, [In] uint iElem, [Out] out double pnVal);

    # 获取属性变体中元素的数量
    [DllImport(Lib.PropSys, CharSet = CharSet.Unicode, SetLastError = true)]
    [return: MarshalAs(UnmanagedType.I4)]
    internal static extern int PropVariantGetElementCount([In] PropVariant propVar);

    # 获取属性变体中文件时间元素
    [DllImport(Lib.PropSys, CharSet = CharSet.Unicode, SetLastError = true, PreserveSig = false)]
    internal static extern void PropVariantGetFileTimeElem([In] PropVariant propVar, [In] uint iElem, [Out, MarshalAs(UnmanagedType.Struct)] out System.Runtime.InteropServices.ComTypes.FILETIME pftVal);

    # 获取属性变体中 16 位整型元素
    [DllImport(Lib.PropSys, CharSet = CharSet.Unicode, SetLastError = true, PreserveSig = false)]
    internal static extern void PropVariantGetInt16Elem([In] PropVariant propVar, [In] uint iElem, [Out] out short pnVal);

    # 获取属性变体中 32 位整型元素
    [DllImport(Lib.PropSys, CharSet = CharSet.Unicode, SetLastError = true, PreserveSig = false)]
    internal static extern void PropVariantGetInt32Elem([In] PropVariant propVar, [In] uint iElem, [Out] out int pnVal);

    # 获取属性变体中 64 位整型元素
    [DllImport(Lib.PropSys, CharSet = CharSet.Unicode, SetLastError = true, PreserveSig = false)]
    internal static extern void PropVariantGetInt64Elem([In] PropVariant propVar, [In] uint iElem, [Out] out long pnVal);

    # 获取属性变体中字符串元素
    [DllImport(Lib.PropSys, CharSet = CharSet.Unicode, SetLastError = true, PreserveSig = false)]
    internal static extern void PropVariantGetStringElem([In] PropVariant propVar, [In] uint iElem, [MarshalAs(UnmanagedType.LPWStr)] ref string ppszVal);

    # 获取属性变体中 16 位无符号整型元素
    [DllImport(Lib.PropSys, CharSet = CharSet.Unicode, SetLastError = true, PreserveSig = false)]
    internal static extern void PropVariantGetUInt16Elem([In] PropVariant propVar, [In] uint iElem, [Out] out ushort pnVal);

    # 获取属性变体中 32 位无符号整型元素
    [DllImport(Lib.PropSys, CharSet = CharSet.Unicode, SetLastError = true, PreserveSig = false)]
    internal static extern void PropVariantGetUInt32Elem([In] PropVariant propVar, [In] uint iElem, [Out] out uint pnVal);

    # 获取属性变体中 64 位无符号整型元素
    [DllImport(Lib.PropSys, CharSet = CharSet.Unicode, SetLastError = true, PreserveSig = false)]
    internal static extern void PropVariantGetUInt64Elem([In] PropVariant propVar, [In] uint iElem, [Out] out ulong pnVal);

    # 访问 SAFEARRAY 数据
    [DllImport(Lib.OleAut32, PreserveSig = false)]
    internal static extern nint SafeArrayAccessData(nint psa);

    # 结束 SAFEARRAY 数据访问
    [DllImport(Lib.OleAut32, PreserveSig = true)]
    # 声明外部函数 SafeArrayCreateVector，用于创建一个指定类型、下界和元素数量的安全数组
    internal static extern nint SafeArrayCreateVector(ushort vt, int lowerBound, uint cElems);

    # 声明外部函数 SafeArrayGetDim，用于获取安全数组的维度数
    [DllImport(Lib.OleAut32, PreserveSig = true)]
    internal static extern uint SafeArrayGetDim(nint psa);

    # 声明外部函数 SafeArrayGetElement，用于获取安全数组中特定索引位置的元素
    [DllImport(Lib.OleAut32, PreserveSig = false)]
    [return: MarshalAs(UnmanagedType.IUnknown)]
    internal static extern object SafeArrayGetElement(nint psa, ref int rgIndices);

    # 声明外部函数 SafeArrayGetLBound，用于获取安全数组指定维度的下界
    [DllImport(Lib.OleAut32, PreserveSig = false)]
    internal static extern int SafeArrayGetLBound(nint psa, uint nDim);

    # 声明外部函数 SafeArrayGetUBound，用于获取安全数组指定维度的上界
    [DllImport(Lib.OleAut32, PreserveSig = false)]
    internal static extern int SafeArrayGetUBound(nint psa, uint nDim);

    # 声明外部函数 SafeArrayUnaccessData，用于释放对安全数组的访问
    [DllImport(Lib.OleAut32, PreserveSig = false)]
    internal static extern void SafeArrayUnaccessData(nint psa);
# 结束一个代码块或控制结构，可能是函数、条件语句、循环等的结束标志
}
```