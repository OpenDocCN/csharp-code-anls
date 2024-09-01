# `.\better-genshin-impact\Build\MicaSetup\Natives\Shell\Dialogs\Interop\Common\ShellCOMInterfaces.cs`

```cs
# 引入 MicaSetup.Natives 命名空间
using MicaSetup.Natives;
# 引入系统相关命名空间
using System;
using System.Runtime.CompilerServices;
using System.Runtime.InteropServices;
using System.Runtime.InteropServices.ComTypes;
using System.Text;

namespace MicaSetup.Shell.Dialogs;

#pragma warning disable CS0108 # 禁用警告 CS0108 (隐藏基类成员)

# 定义 SICHINTF 枚举，用于表示不同的条件提示类型
public enum SICHINTF
{
    # 显示条件
    SICHINT_DISPLAY = 0x00000000,
    # 规范化条件
    SICHINT_CANONICAL = 0x10000000,
    # 测试文件路径是否不相等
    SICHINT_TEST_FILESYSPATH_IF_NOT_EQUAL = 0x20000000,
    # 所有字段
    SICHINT_ALLFIELDS = unchecked((int)0x80000000)
}

# 定义 ICondition 接口，表示条件对象，具有持久化流的能力
[ComImport(),
Guid(ShellIIDGuid.ICondition),
InterfaceType(ComInterfaceType.InterfaceIsIUnknown)]
public interface ICondition : IPersistStream
{
    # 获取类 ID
    [PreserveSig]
    void GetClassID(out Guid pClassID);

    # 判断对象是否已被修改
    [PreserveSig]
    HResult IsDirty();

    # 从流加载对象
    [PreserveSig]
    HResult Load([In, MarshalAs(UnmanagedType.Interface)] IStream stm);

    # 保存对象到流
    [PreserveSig]
    HResult Save([In, MarshalAs(UnmanagedType.Interface)] IStream stm, bool fRemember);

    # 获取对象的最大大小
    [PreserveSig]
    HResult GetSizeMax(out ulong cbSize);

    # 获取条件类型
    [PreserveSig]
    HResult GetConditionType([Out()] out SearchConditionType pNodeType);

    # 获取子条件
    [PreserveSig]
    HResult GetSubConditions([In] ref Guid riid, [Out, MarshalAs(UnmanagedType.Interface)] out object ppv);

    # 获取比较信息
    [PreserveSig]
    HResult GetComparisonInfo(
        [Out, MarshalAs(UnmanagedType.LPWStr)] out string ppszPropertyName,
        [Out] out SearchConditionOperation pcop,
        [Out] PropVariant ppropvar);

    # 获取值类型名称
    [PreserveSig]
    HResult GetValueType([Out, MarshalAs(UnmanagedType.LPWStr)] out string ppszValueTypeName);

    # 获取值规范化
    [PreserveSig]
    HResult GetValueNormalization([Out, MarshalAs(UnmanagedType.LPWStr)] out string ppszNormalization);

    # 获取输入术语
    [PreserveSig]
    HResult GetInputTerms([Out] out IRichChunk ppPropertyTerm, [Out] out IRichChunk ppOperationTerm, [Out] out IRichChunk ppValueTerm);

    # 克隆条件对象
    [PreserveSig]
    HResult Clone([Out()] out ICondition ppc);
};

# 定义 IConditionFactory 接口，用于创建不同类型的条件对象
[ComImport,
Guid(ShellIIDGuid.IConditionFactory),
InterfaceType(ComInterfaceType.InterfaceIsIUnknown)]
public interface IConditionFactory
{
    # 创建一个 NOT 条件
    [PreserveSig]
    HResult MakeNot([In] ICondition pcSub, [In] bool fSimplify, [Out] out ICondition ppcResult);

    # 创建一个 AND 或 OR 条件
    [PreserveSig]
    HResult MakeAndOr([In] SearchConditionType ct, [In] IEnumUnknown peuSubs, [In] bool fSimplify, [Out] out ICondition ppcResult);

    # 创建一个叶子条件
    [PreserveSig]
    HResult MakeLeaf(
        [In, MarshalAs(UnmanagedType.LPWStr)] string pszPropertyName,
        [In] SearchConditionOperation cop,
        [In, MarshalAs(UnmanagedType.LPWStr)] string pszValueType,
        [In] PropVariant ppropvar,
        IRichChunk richChunk1,
        IRichChunk richChunk2,
        IRichChunk richChunk3,
        [In] bool fExpand,
        [Out] out ICondition ppcResult);

    # 解析条件
    [PreserveSig]
    HResult Resolve();
};

# 定义 IEntity 接口，代表一个实体（接口体为空）
[ComImport,
Guid("24264891-E80B-4fd3-B7CE-4FF2FAE8931F"),
InterfaceType(ComInterfaceType.InterfaceIsIUnknown)]
public interface IEntity
{
}

# 定义 IEnumIDList 接口，用于枚举 ID 列表
[ComImport,
Guid(ShellIIDGuid.IEnumIDList),
InterfaceType(ComInterfaceType.InterfaceIsIUnknown)]
public interface IEnumIDList
{
    # 保留方法签名，表示该方法的原始签名应该保留
    [PreserveSig]
    # 指定该方法是内部调用的，并使用运行时代码类型
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    # 定义了 Next 方法，用于获取指定数量的元素，并输出实际获取的元素数量
    HResult Next(uint celt, out nint rgelt, out uint pceltFetched);
    
    # 保留方法签名，表示该方法的原始签名应该保留
    [PreserveSig]
    # 指定该方法是内部调用的，并使用运行时代码类型
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    # 定义了 Skip 方法，用于跳过指定数量的元素
    HResult Skip([In] uint celt);
    
    # 保留方法签名，表示该方法的原始签名应该保留
    [PreserveSig]
    # 指定该方法是内部调用的，并使用运行时代码类型
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    # 定义了 Reset 方法，用于重置迭代器到初始状态
    HResult Reset();
    
    # 保留方法签名，表示该方法的原始签名应该保留
    [PreserveSig]
    # 指定该方法是内部调用的，并使用运行时代码类型
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    # 定义了 Clone 方法，用于克隆当前迭代器并返回一个新的 IEnumIDList 实例
    HResult Clone([MarshalAs(UnmanagedType.Interface)] out IEnumIDList ppenum);
}
# 结束当前代码块或类

[ComImport]
# 标记接口为 COM 导入的接口
[InterfaceType(ComInterfaceType.InterfaceIsIUnknown)]
# 指定接口的 COM GUID
[Guid(ShellIIDGuid.IEnumUnknown)]
public interface IEnumUnknown
{
    [PreserveSig]
    # 指定方法的返回值应由调用者处理
    HResult Next(uint requestedNumber, ref nint buffer, ref uint fetchedNumber);

    [PreserveSig]
    # 指定方法的返回值应由调用者处理
    HResult Skip(uint number);

    [PreserveSig]
    # 指定方法的返回值应由调用者处理
    HResult Reset();

    [PreserveSig]
    # 指定方法的返回值应由调用者处理
    HResult Clone(out IEnumUnknown result);
}

[ComImport(),
                    Guid(ShellIIDGuid.IModalWindow),
# 标记接口为 COM 导入的接口，并指定其 GUID 和接口类型
InterfaceType(ComInterfaceType.InterfaceIsIUnknown)]
public interface IModalWindow
{
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime),
    PreserveSig]
    # 指定方法的实现方式和返回值处理
    int Show([In] nint parent);
}

[ComImport,
# 标记接口为 COM 导入的接口，并指定其 GUID 和 CoClass 类型
Guid(ShellIIDGuid.IConditionFactory),
CoClass(typeof(ConditionFactoryCoClass))]
public interface INativeConditionFactory : IConditionFactory
{
}

[ComImport,
# 标记接口为 COM 导入的接口，并指定其 GUID 和 CoClass 类型
Guid(ShellIIDGuid.IQueryParserManager),
CoClass(typeof(QueryParserManagerCoClass))]
public interface INativeQueryParserManager : IQueryParserManager
{
}

[ComImport,
# 标记接口为 COM 导入的接口，并指定其 GUID 和 CoClass 类型
Guid(ShellIIDGuid.ISearchFolderItemFactory),
CoClass(typeof(SearchFolderItemFactoryCoClass))]
public interface INativeSearchFolderItemFactory : ISearchFolderItemFactory
{
}

[ComImport]
# 标记接口为 COM 导入的接口
[InterfaceType(ComInterfaceType.InterfaceIsIUnknown)]
# 指定接口的 COM GUID
[Guid("00000109-0000-0000-C000-000000000046")]
public interface IPersistStream
{
    [PreserveSig]
    # 指定方法的返回值应由调用者处理
    void GetClassID(out Guid pClassID);

    [PreserveSig]
    # 指定方法的返回值应由调用者处理
    HResult IsDirty();

    [PreserveSig]
    # 指定方法的返回值应由调用者处理
    HResult Load([In, MarshalAs(UnmanagedType.Interface)] IStream stm);

    [PreserveSig]
    # 指定方法的返回值应由调用者处理
    HResult Save([In, MarshalAs(UnmanagedType.Interface)] IStream stm, bool fRemember);

    [PreserveSig]
    # 指定方法的返回值应由调用者处理
    HResult GetSizeMax(out ulong cbSize);
}

[ComImport,
# 标记接口为 COM 导入的接口，并指定其 GUID 和接口类型
Guid(ShellIIDGuid.IQueryParser),
InterfaceType(ComInterfaceType.InterfaceIsIUnknown)]
public interface IQueryParser
{
    [PreserveSig]
    # 指定方法的返回值应由调用者处理
    HResult Parse([In, MarshalAs(UnmanagedType.LPWStr)] string pszInputString, [In] IEnumUnknown pCustomProperties, [Out] out IQuerySolution ppSolution);

    [PreserveSig]
    # 指定方法的返回值应由调用者处理
    HResult SetOption([In] StructuredQuerySingleOption option, [In] PropVariant pOptionValue);

    [PreserveSig]
    # 指定方法的返回值应由调用者处理
    HResult GetOption([In] StructuredQuerySingleOption option, [Out] PropVariant pOptionValue);

    [PreserveSig]
    # 指定方法的返回值应由调用者处理
    HResult SetMultiOption([In] StructuredQueryMultipleOption option, [In, MarshalAs(UnmanagedType.LPWStr)] string pszOptionKey, [In] PropVariant pOptionValue);

    [PreserveSig]
    # 指定方法的返回值应由调用者处理
    HResult GetSchemaProvider([Out] out nint ppSchemaProvider);

    [PreserveSig]
    # 指定方法的返回值应由调用者处理
    HResult RestateToString([In] ICondition pCondition, [In] bool fUseEnglish, [Out, MarshalAs(UnmanagedType.LPWStr)] out string ppszQueryString);

    [PreserveSig]
    # 指定方法的返回值应由调用者处理
    HResult ParsePropertyValue([In, MarshalAs(UnmanagedType.LPWStr)] string pszPropertyName, [In, MarshalAs(UnmanagedType.LPWStr)] string pszInputString, [Out] out IQuerySolution ppSolution);

    [PreserveSig]
    # 指定方法的返回值应由调用者处理
    # 定义一个方法，用于将属性值重新表示为字符串
    HResult RestatePropertyValueToString([In] ICondition pCondition, [In] bool fUseEnglish, [Out, MarshalAs(UnmanagedType.LPWStr)] out string ppszPropertyName, [Out, MarshalAs(UnmanagedType.LPWStr)] out string ppszQueryString);
    # [In] 指定 pCondition 参数为输入参数，类型为 ICondition
    # [In] 指定 fUseEnglish 参数为输入参数，类型为 bool
    # [Out, MarshalAs(UnmanagedType.LPWStr)] 指定 ppszPropertyName 参数为输出参数，并按照 LPWSTR 类型进行 Marshal
    # [Out, MarshalAs(UnmanagedType.LPWStr)] 指定 ppszQueryString 参数为输出参数，并按照 LPWSTR 类型进行 Marshal
}



# 指定接口的 COM 导入特性和 GUID，并定义接口类型为 IUnknown
[ComImport,
Guid(ShellIIDGuid.IQueryParserManager),
InterfaceType(ComInterfaceType.InterfaceIsIUnknown)]
public interface IQueryParserManager
{
    # 方法用于创建加载的查询解析器，返回 HRESULT 状态
    [PreserveSig]
    HResult CreateLoadedParser([In, MarshalAs(UnmanagedType.LPWStr)] string pszCatalog, [In] ushort langidForKeywords, [In] ref Guid riid, [Out] out IQueryParser ppQueryParser);

    # 方法用于初始化查询解析器选项，返回 HRESULT 状态
    [PreserveSig]
    HResult InitializeOptions([In] bool fUnderstandNQS, [In] bool fAutoWildCard, [In] IQueryParser pQueryParser);

    # 方法用于设置查询解析器的选项，返回 HRESULT 状态
    [PreserveSig]
    HResult SetOption([In] QueryParserManagerOption option, [In] PropVariant pOptionValue);
};

# 指定接口的 COM 导入特性和 GUID，并定义接口类型为 IUnknown
[ComImport,
Guid(ShellIIDGuid.IQuerySolution),
InterfaceType(ComInterfaceType.InterfaceIsIUnknown)]
public interface IQuerySolution : IConditionFactory
{
    # 方法用于创建一个 NOT 条件，返回 HRESULT 状态
    [PreserveSig]
    HResult MakeNot([In] ICondition pcSub, [In] bool fSimplify, [Out] out ICondition ppcResult);

    # 方法用于创建 AND 或 OR 条件，返回 HRESULT 状态
    [PreserveSig]
    HResult MakeAndOr([In] SearchConditionType ct, [In] IEnumUnknown peuSubs, [In] bool fSimplify, [Out] out ICondition ppcResult);

    # 方法用于创建叶节点条件，返回 HRESULT 状态
    [PreserveSig]
    HResult MakeLeaf(
        [In, MarshalAs(UnmanagedType.LPWStr)] string pszPropertyName,
        [In] SearchConditionOperation cop,
        [In, MarshalAs(UnmanagedType.LPWStr)] string pszValueType,
        [In] PropVariant ppropvar,
        IRichChunk richChunk1,
        IRichChunk richChunk2,
        IRichChunk richChunk3,
        [In] bool fExpand,
        [Out] out ICondition ppcResult);

    # 方法用于解析条件，返回 HRESULT 状态
    [PreserveSig]
    HResult Resolve();

    # 方法用于获取查询节点和主要实体，返回 HRESULT 状态
    [PreserveSig]
    HResult GetQuery([Out, MarshalAs(UnmanagedType.Interface)] out ICondition ppQueryNode, [Out, MarshalAs(UnmanagedType.Interface)] out IEntity ppMainType);

    # 方法用于获取解析错误，返回 HRESULT 状态
    [PreserveSig]
    HResult GetErrors([In] ref Guid riid, [Out] out nint ppParseErrors);

    # 方法用于获取词法数据，返回 HRESULT 状态
    [PreserveSig]
    HResult GetLexicalData([MarshalAs(UnmanagedType.LPWStr)] out string ppszInputString, [Out] out nint ppTokens, [Out] out uint plcid, [Out] /* IUnknown** */ out nint ppWordBreaker);
}

# 指定接口的 COM 导入特性和 GUID，并定义接口类型为 IUnknown
[ComImport,
Guid(ShellIIDGuid.IRichChunk),
InterfaceType(ComInterfaceType.InterfaceIsIUnknown)]
public interface IRichChunk
{
    # 方法用于获取数据，返回 HRESULT 状态
    [PreserveSig]
    HResult GetData();
}

# 指定接口的 COM 导入特性和 GUID，并定义接口类型为 IUnknown
[ComImport,
Guid(ShellIIDGuid.ISearchFolderItemFactory),
InterfaceType(ComInterfaceType.InterfaceIsIUnknown)]
public interface ISearchFolderItemFactory
{
    # 方法用于设置搜索范围，返回 HRESULT 状态
    [PreserveSig]
    HResult SetScope([In, MarshalAs(UnmanagedType.Interface)] IShellItemArray ppv);

    # 方法用于设置条件，返回 HRESULT 状态
    [PreserveSig]
    HResult SetCondition([In] ICondition pCondition);

    # 方法用于获取 Shell 项，返回 HRESULT 状态
    [PreserveSig]
    int GetShellItem(ref Guid riid, [Out, MarshalAs(UnmanagedType.Interface)] out IShellItem ppv);
};

# 指定接口的 COM 导入特性和 GUID，并定义接口类型为 IUnknown
[ComImport,
Guid(ShellIIDGuid.ISharedBitmap),
InterfaceType(ComInterfaceType.InterfaceIsIUnknown)]
public interface ISharedBitmap
{
    # 方法用于获取共享位图句柄
    void GetSharedBitmap([Out] out nint phbm);

    # 方法用于获取位图的大小
    void GetSize([Out] out SIZE pSize);

    # 方法用于获取位图的格式
    void GetFormat([Out] out ThumbnailAlphaType pat);

    # 方法用于初始化位图
    void InitializeBitmap([In] nint hbm, [In] ThumbnailAlphaType wtsAT);

    # 方法用于分离位图句柄
    void Detach([Out] out nint phbm);
}

[ComImport,
# 定义 IShellFolder 接口，用于操作 Shell 文件夹对象
[Guid(ShellIIDGuid.IShellFolder),
InterfaceType(ComInterfaceType.InterfaceIsIUnknown),
ComConversionLoss]
public interface IShellFolder
{
    # 解析显示名称，返回相应的 PIDL（项目标识符列表）及其他信息
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    void ParseDisplayName(nint hwnd, [In, MarshalAs(UnmanagedType.Interface)] IBindCtx pbc, [In, MarshalAs(UnmanagedType.LPWStr)] string pszDisplayName, [In, Out] ref uint pchEaten, [Out] nint ppidl, [In, Out] ref uint pdwAttributes);

    # 枚举 Shell 文件夹中的对象
    [PreserveSig]
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    HResult EnumObjects([In] nint hwnd, [In] ShellFolderEnumerationOptions grfFlags, [MarshalAs(UnmanagedType.Interface)] out IEnumIDList ppenumIDList);

    # 绑定到指定的对象并返回相应的 IShellFolder 接口
    [PreserveSig]
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    HResult BindToObject([In] nint pidl, nint pbc, [In] ref Guid riid, [Out, MarshalAs(UnmanagedType.Interface)] out IShellFolder ppv);

    # 绑定到存储对象
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    void BindToStorage([In] ref nint pidl, [In, MarshalAs(UnmanagedType.Interface)] IBindCtx pbc, [In] ref Guid riid, out nint ppv);

    # 比较两个 PIDL 对象的 ID
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    void CompareIDs([In] nint lParam, [In] ref nint pidl1, [In] ref nint pidl2);

    # 创建视图对象并返回其接口
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    void CreateViewObject([In] nint hwndOwner, [In] ref Guid riid, out nint ppv);

    # 获取指定对象的属性
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    void GetAttributesOf([In] uint cidl, [In] nint apidl, [In, Out] ref uint rgfInOut);

    # 获取 UI 对象的接口
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    void GetUIObjectOf([In] nint hwndOwner, [In] uint cidl, [In] nint apidl, [In] ref Guid riid, [In, Out] ref uint rgfReserved, out nint ppv);

    # 获取显示名称
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    void GetDisplayNameOf([In] ref nint pidl, [In] uint uFlags, out nint pName);

    # 设置显示名称
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    void SetNameOf([In] nint hwnd, [In] ref nint pidl, [In, MarshalAs(UnmanagedType.LPWStr)] string pszName, [In] uint uFlags, [Out] nint ppidlOut);
}

# 定义 IShellFolder2 接口，继承自 IShellFolder，增加了新的功能
[ComImport,
Guid(ShellIIDGuid.IShellFolder2),
InterfaceType(ComInterfaceType.InterfaceIsIUnknown),
ComConversionLoss]
public interface IShellFolder2 : IShellFolder
{
    # 解析显示名称，返回相应的 PIDL（项目标识符列表）及其他信息
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    void ParseDisplayName([In] nint hwnd, [In, MarshalAs(UnmanagedType.Interface)] IBindCtx pbc, [In, MarshalAs(UnmanagedType.LPWStr)] string pszDisplayName, [In, Out] ref uint pchEaten, [Out] nint ppidl, [In, Out] ref uint pdwAttributes);

    # 尚未完成的方法定义
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    # 枚举对象的方法，传入窗口句柄、Shell 文件夹枚举选项，并返回枚举接口
    void EnumObjects([In] nint hwnd, [In] ShellFolderEnumerationOptions grfFlags, [MarshalAs(UnmanagedType.Interface)] out IEnumIDList ppenumIDList);

    # 绑定到对象的方法，传入 PIDL、绑定上下文、接口 ID，返回绑定到的 Shell 文件夹接口
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    void BindToObject([In] nint pidl, nint pbc, [In] ref Guid riid, [Out, MarshalAs(UnmanagedType.Interface)] out IShellFolder ppv);

    # 绑定到存储的方法，传入 PIDL、绑定上下文、接口 ID，返回绑定到的存储接口
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    void BindToStorage([In] ref nint pidl, [In, MarshalAs(UnmanagedType.Interface)] IBindCtx pbc, [In] ref Guid riid, out nint ppv);

    # 比较 ID 方法，传入参数和两个 PIDL 进行比较
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    void CompareIDs([In] nint lParam, [In] ref nint pidl1, [In] ref nint pidl2);

    # 创建视图对象的方法，传入窗口句柄和接口 ID，返回视图对象
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    void CreateViewObject([In] nint hwndOwner, [In] ref Guid riid, out nint ppv);

    # 获取属性的方法，传入文件夹项数目、文件夹项列表，并返回属性
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    void GetAttributesOf([In] uint cidl, [In] nint apidl, [In, Out] ref uint rgfInOut);

    # 获取用户界面对象的方法，传入窗口句柄、文件夹项数目、文件夹项列表、接口 ID，并返回界面对象
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    void GetUIObjectOf([In] nint hwndOwner, [In] uint cidl, [In] nint apidl, [In] ref Guid riid, [In, Out] ref uint rgfReserved, out nint ppv);

    # 获取显示名称的方法，传入 PIDL 和标志，并返回显示名称
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    void GetDisplayNameOf([In] ref nint pidl, [In] uint uFlags, out nint pName);

    # 设置名称的方法，传入窗口句柄、PIDL、新名称、标志，并返回新的 PIDL
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    void SetNameOf([In] nint hwnd, [In] ref nint pidl, [In, MarshalAs(UnmanagedType.LPWStr)] string pszName, [In] uint uFlags, [Out] nint ppidlOut);

    # 获取默认搜索 GUID 的方法
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    void GetDefaultSearchGUID(out Guid pguid);

    # 枚举搜索的方法，返回搜索枚举接口
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    void EnumSearches([Out] out nint ppenum);

    # 获取默认列的方法，传入资源 ID，并返回排序和显示列
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    void GetDefaultColumn([In] uint dwRes, out uint pSort, out uint pDisplay);

    # 获取默认列状态的方法，传入列索引，并返回列状态标志
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    void GetDefaultColumnState([In] uint iColumn, out uint pcsFlags);

    # 获取详细信息的方法，传入 PIDL 和属性键，并返回详细信息
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    void GetDetailsEx([In] ref nint pidl, [In] ref PropertyKey pscid, [MarshalAs(UnmanagedType.Struct)] out object pv);

    # 获取详细信息的方法，传入 PIDL 和列索引，并返回详细信息描述
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    void GetDetailsOf([In] ref nint pidl, [In] uint iColumn, out nint psd);

    # 将列索引映射到属性键的方法
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    void MapColumnToSCID([In] uint iColumn, out PropertyKey pscid);
}

# ComImport 特性指示该接口是从 COM 导入的
[ComImport,
                                                # IShellItem 接口的 GUID
                                                Guid(ShellIIDGuid.IShellItem),
                                                # 指定接口的 COM 类型为 IUnknown
InterfaceType(ComInterfaceType.InterfaceIsIUnknown)]
public interface IShellItem
{
    # 保留签名，指定方法实现方式为内部调用
    [PreserveSig]
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    # 绑定到指定的处理程序
    HResult BindToHandler(
        [In] nint pbc,
        [In] ref Guid bhid,
        [In] ref Guid riid,
        [Out, MarshalAs(UnmanagedType.Interface)] out IShellFolder ppv);

    # 获取父项
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    void GetParent([MarshalAs(UnmanagedType.Interface)] out IShellItem ppsi);

    # 保留签名，获取显示名称
    [PreserveSig]
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    HResult GetDisplayName(
        [In] ShellItemDesignNameOptions sigdnName,
        out nint ppszName);

    # 获取属性
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    void GetAttributes([In] ShellFileGetAttributesOptions sfgaoMask, out ShellFileGetAttributesOptions psfgaoAttribs);

    # 保留签名，比较项
    [PreserveSig]
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    HResult Compare(
        [In, MarshalAs(UnmanagedType.Interface)] IShellItem psi,
        [In] SICHINTF hint,
        out int piOrder);
}

# ComImport 特性指示该接口是从 COM 导入的
[ComImport,
# IShellItem2 接口的 GUID
Guid(ShellIIDGuid.IShellItem2),
# 指定接口的 COM 类型为 IUnknown
InterfaceType(ComInterfaceType.InterfaceIsIUnknown)]
public interface IShellItem2 : IShellItem
{
    # 保留签名，绑定到指定的处理程序
    [PreserveSig]
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    HResult BindToHandler(
        [In] nint pbc,
        [In] ref Guid bhid,
        [In] ref Guid riid,
        [Out, MarshalAs(UnmanagedType.Interface)] out IShellFolder ppv);

    # 保留签名，获取父项
    [PreserveSig]
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    HResult GetParent([MarshalAs(UnmanagedType.Interface)] out IShellItem ppsi);

    # 保留签名，获取显示名称
    [PreserveSig]
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    HResult GetDisplayName(
        [In] ShellItemDesignNameOptions sigdnName,
        [MarshalAs(UnmanagedType.LPWStr)] out string ppszName);

    # 获取属性
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    void GetAttributes([In] ShellFileGetAttributesOptions sfgaoMask, out ShellFileGetAttributesOptions psfgaoAttribs);

    # 获取比较项的详细信息
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    void Compare(
        [In, MarshalAs(UnmanagedType.Interface)] IShellItem psi,
        [In] uint hint,
        out int piOrder);

    # 获取属性存储
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime), PreserveSig]
    int GetPropertyStore(
        [In] GetPropertyStoreOptions Flags,
        [In] ref Guid riid,
        [Out, MarshalAs(UnmanagedType.Interface)] out IPropertyStore ppv);

    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    # 定义一个方法，用于获取属性存储对象并在需要时创建对象
    void GetPropertyStoreWithCreateObject([In] GetPropertyStoreOptions Flags, [In, MarshalAs(UnmanagedType.IUnknown)] object punkCreateObject, [In] ref Guid riid, out nint ppv);

    # 定义一个方法，用于根据指定的属性键获取属性存储，并将其作为 IPropertyStore 对象返回
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    void GetPropertyStoreForKeys([In] ref PropertyKey rgKeys, [In] uint cKeys, [In] GetPropertyStoreOptions Flags, [In] ref Guid riid, [Out, MarshalAs(UnmanagedType.IUnknown)] out IPropertyStore ppv);

    # 定义一个方法，用于获取属性描述列表，基于给定的属性键和接口标识符
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    void GetPropertyDescriptionList([In] ref PropertyKey keyType, [In] ref Guid riid, out nint ppv);

    # 定义一个方法，用于更新对象状态，传入绑定上下文
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    HResult Update([In, MarshalAs(UnmanagedType.Interface)] IBindCtx pbc);

    # 定义一个方法，用于获取属性值，并将结果存储在 PropVariant 对象中
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    void GetProperty([In] ref PropertyKey key, [Out] PropVariant ppropvar);

    # 定义一个方法，用于获取 CLSID（类标识符）值，基于指定的属性键
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    void GetCLSID([In] ref PropertyKey key, out Guid pclsid);

    # 定义一个方法，用于获取文件时间，基于指定的属性键
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    void GetFileTime([In] ref PropertyKey key, out System.Runtime.InteropServices.ComTypes.FILETIME pft);

    # 定义一个方法，用于获取 32 位整数值，基于指定的属性键
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    void GetInt32([In] ref PropertyKey key, out int pi);

    # 定义一个方法，用于获取字符串值，基于指定的属性键，结果用 LPWSTR（宽字符字符串）表示
    [PreserveSig]
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    HResult GetString([In] ref PropertyKey key, [MarshalAs(UnmanagedType.LPWStr)] out string ppsz);

    # 定义一个方法，用于获取 32 位无符号整数值，基于指定的属性键
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    void GetUInt32([In] ref PropertyKey key, out uint pui);

    # 定义一个方法，用于获取 64 位无符号整数值，基于指定的属性键
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    void GetUInt64([In] ref PropertyKey key, out ulong pull);

    # 定义一个方法，用于获取布尔值，基于指定的属性键，结果用整数表示
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    void GetBool([In] ref PropertyKey key, out int pf);
# 定义 IShellItemArray 接口，提供与 Shell 项目数组的交互功能
[ComImport,
Guid(ShellIIDGuid.IShellItemArray),
InterfaceType(ComInterfaceType.InterfaceIsIUnknown)]
public interface IShellItemArray
{
    # 绑定到指定的处理程序
    [PreserveSig]
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    HResult BindToHandler(
        [In, MarshalAs(UnmanagedType.Interface)] nint pbc,
        [In] ref Guid rbhid,
        [In] ref Guid riid,
        out nint ppvOut);

    # 获取属性存储
    [PreserveSig]
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    HResult GetPropertyStore(
        [In] int Flags,
        [In] ref Guid riid,
        out nint ppv);

    # 获取属性描述列表
    [PreserveSig]
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    HResult GetPropertyDescriptionList(
        [In] ref PropertyKey keyType,
        [In] ref Guid riid,
        out nint ppv);

    # 获取属性
    [PreserveSig]
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    HResult GetAttributes(
        [In] ShellItemAttributeOptions dwAttribFlags,
        [In] ShellFileGetAttributesOptions sfgaoMask,
        out ShellFileGetAttributesOptions psfgaoAttribs);

    # 获取项目数量
    [PreserveSig]
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    HResult GetCount(out uint pdwNumItems);

    # 获取指定索引的项目
    [PreserveSig]
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    HResult GetItemAt(
        [In] uint dwIndex,
        [MarshalAs(UnmanagedType.Interface)] out IShellItem ppsi);

    # 枚举所有项目
    [PreserveSig]
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    HResult EnumItems([MarshalAs(UnmanagedType.Interface)] out nint ppenumShellItems);
}

# 定义 IShellItemImageFactory 接口，提供获取 Shell 项目图像的功能
[ComImportAttribute()]
[GuidAttribute("bcc18b79-ba16-442f-80c4-8a59c30c463b")]
[InterfaceTypeAttribute(ComInterfaceType.InterfaceIsIUnknown)]
public interface IShellItemImageFactory
{
    # 获取指定大小和标志的图像
    [PreserveSig]
    HResult GetImage(
    [In, MarshalAs(UnmanagedType.Struct)] SIZE size,
    [In] SIIGBF flags,
    [Out] out nint phbm);
}

# 定义 IShellLibrary 接口，提供与 Shell 库的交互功能
[ComImport,
    Guid(ShellIIDGuid.IShellLibrary),
InterfaceType(ComInterfaceType.InterfaceIsIUnknown)]
public interface IShellLibrary
{
    # 从 Shell 项目加载库
    [PreserveSig]
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    HResult LoadLibraryFromItem(
        [In, MarshalAs(UnmanagedType.Interface)] IShellItem library,
        [In] AccessModes grfMode);

    # 从已知文件夹加载库
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    void LoadLibraryFromKnownFolder(
        [In] ref Guid knownfidLibrary,
        [In] AccessModes grfMode);

    # 添加文件夹到库
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    void AddFolder([In, MarshalAs(UnmanagedType.Interface)] IShellItem location);

    # 其他方法未显示
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    # 从指定的 IShellItem 位置移除文件夹
    void RemoveFolder([In, MarshalAs(UnmanagedType.Interface)] IShellItem location);

    # 通过指定的 LibraryFolderFilter 获取文件夹
    [PreserveSig]
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    HResult GetFolders(
        [In] LibraryFolderFilter lff,  # 过滤器，用于筛选文件夹
        [In] ref Guid riid,  # 请求的接口 ID
        [MarshalAs(UnmanagedType.Interface)] out IShellItemArray ppv);  # 输出的文件夹数组接口

    # 解析指定的文件夹，可能会进行超时等待
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    void ResolveFolder(
        [In, MarshalAs(UnmanagedType.Interface)] IShellItem folderToResolve,  # 需要解析的文件夹
        [In] uint timeout,  # 超时时间
        [In] ref Guid riid,  # 请求的接口 ID
        [MarshalAs(UnmanagedType.Interface)] out IShellItem ppv);  # 输出的解析结果文件夹接口

    # 获取默认保存文件夹
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    void GetDefaultSaveFolder(
        [In] DefaultSaveFolderType dsft,  # 默认保存文件夹类型
        [In] ref Guid riid,  # 请求的接口 ID
        [MarshalAs(UnmanagedType.Interface)] out IShellItem ppv);  # 输出的默认保存文件夹接口

    # 设置默认保存文件夹
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    void SetDefaultSaveFolder(
        [In] DefaultSaveFolderType dsft,  # 默认保存文件夹类型
        [In, MarshalAs(UnmanagedType.Interface)] IShellItem si);  # 需要设置的文件夹接口

    # 获取库的选项设置
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    void GetOptions(
        out LibraryOptions lofOptions);  # 输出的库选项设置

    # 设置库的选项
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    void SetOptions(
        [In] LibraryOptions lofMask,  # 要修改的选项掩码
        [In] LibraryOptions lofOptions);  # 新的选项设置

    # 获取文件夹类型 ID
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    void GetFolderType(out Guid ftid);  # 输出的文件夹类型 ID

    # 设置文件夹类型 ID
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    void SetFolderType([In] ref Guid ftid);  # 需要设置的文件夹类型 ID

    # 获取图标
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    void GetIcon([MarshalAs(UnmanagedType.LPWStr)] out string icon);  # 输出的图标字符串

    # 设置图标
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    void SetIcon([In, MarshalAs(UnmanagedType.LPWStr)] string icon);  # 需要设置的图标字符串

    # 提交更改
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    void Commit();

    # 将库保存到指定的文件夹中
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    void Save(
        [In, MarshalAs(UnmanagedType.Interface)] IShellItem folderToSaveIn,  # 需要保存到的文件夹接口
        [In, MarshalAs(UnmanagedType.LPWStr)] string libraryName,  # 要保存的库名称
        [In] LibrarySaveOptions lsf,  # 保存选项
        [MarshalAs(UnmanagedType.Interface)] out IShellItem2 savedTo);  # 输出的保存结果接口

    # 将库保存到已知文件夹中
    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    void SaveInKnownFolder(
        [In] ref Guid kfidToSaveIn,  # 需要保存到的已知文件夹 ID
        [In, MarshalAs(UnmanagedType.LPWStr)] string libraryName,  # 要保存的库名称
        [In] LibrarySaveOptions lsf,  # 保存选项
        [MarshalAs(UnmanagedType.Interface)] out IShellItem2 savedTo);  # 输出的保存结果接口
# 结束前一个代码块
};

# 声明 IShellLinkW 接口，该接口用于处理 Shell 链接的操作
[ComImport,
Guid(ShellIIDGuid.IShellLinkW),
InterfaceType(ComInterfaceType.InterfaceIsIUnknown)]
public interface IShellLinkW
{
    # 获取链接的路径
    void GetPath(
        [Out(), MarshalAs(UnmanagedType.LPWStr)] StringBuilder pszFile,
        int cchMaxPath,
        nint pfd,
        uint fFlags);

    # 获取链接的 ID 列表
    void GetIDList(out nint ppidl);

    # 设置链接的 ID 列表
    void SetIDList(nint pidl);

    # 获取链接的描述
    void GetDescription(
        [Out(), MarshalAs(UnmanagedType.LPWStr)] StringBuilder pszFile,
        int cchMaxName);

    # 设置链接的描述
    void SetDescription(
        [MarshalAs(UnmanagedType.LPWStr)] string pszName);

    # 获取链接的工作目录
    void GetWorkingDirectory(
        [Out(), MarshalAs(UnmanagedType.LPWStr)] StringBuilder pszDir,
        int cchMaxPath
        );

    # 设置链接的工作目录
    void SetWorkingDirectory(
        [MarshalAs(UnmanagedType.LPWStr)] string pszDir);

    # 获取链接的参数
    void GetArguments(
        [Out(), MarshalAs(UnmanagedType.LPWStr)] StringBuilder pszArgs,
        int cchMaxPath);

    # 设置链接的参数
    void SetArguments(
        [MarshalAs(UnmanagedType.LPWStr)] string pszArgs);

    # 获取链接的热键
    void GetHotKey(out short wHotKey);

    # 设置链接的热键
    void SetHotKey(short wHotKey);

    # 获取链接的显示命令
    void GetShowCmd(out uint iShowCmd);

    # 设置链接的显示命令
    void SetShowCmd(uint iShowCmd);

    # 获取链接的图标位置
    void GetIconLocation(
        [Out(), MarshalAs(UnmanagedType.LPWStr)] out StringBuilder pszIconPath,
        int cchIconPath,
        out int iIcon);

    # 设置链接的图标位置
    void SetIconLocation(
        [MarshalAs(UnmanagedType.LPWStr)] string pszIconPath,
        int iIcon);

    # 设置链接的相对路径
    void SetRelativePath(
        [MarshalAs(UnmanagedType.LPWStr)] string pszPathRel,
        uint dwReserved);

    # 解析链接
    void Resolve(nint hwnd, uint fFlags);

    # 设置链接的路径
    void SetPath(
        [MarshalAs(UnmanagedType.LPWStr)] string pszFile);
}

# 声明 IThumbnailCache 接口，该接口用于获取缩略图
[ComImport,
    Guid(ShellIIDGuid.IThumbnailCache),
InterfaceType(ComInterfaceType.InterfaceIsIUnknown)]
public interface IThumbnailCache
{
    # 获取缩略图
    void GetThumbnail([In] IShellItem pShellItem,
    [In] uint cxyRequestedThumbSize,
    [In] ThumbnailOptions flags,
    [Out] out ISharedBitmap ppvThumb,
    [Out] out ThumbnailCacheOptions pOutFlags,
    [Out] ThumbnailId pThumbnailID);

    # 根据 ID 获取缩略图
    void GetThumbnailByID([In] ThumbnailId thumbnailID,
    [In] uint cxyRequestedThumbSize,
    [Out] out ISharedBitmap ppvThumb,
    [Out] out ThumbnailCacheOptions pOutFlags);
}

# 声明 ConditionFactoryCoClass 类，作为条件工厂的 COM 类
[ComImport,
ClassInterface(ClassInterfaceType.None),
TypeLibType(TypeLibTypeFlags.FCanCreate),
Guid(ShellCLSIDGuid.ConditionFactory)]
public class ConditionFactoryCoClass
{
}

# 声明 CShellLink 类，作为 Shell 链接的 COM 类
[ComImport,
    Guid(ShellIIDGuid.CShellLink),
ClassInterface(ClassInterfaceType.None)]
public class CShellLink { }

# 声明 QueryParserManagerCoClass 类，作为查询解析器管理器的 COM 类
[ComImport,
ClassInterface(ClassInterfaceType.None),
TypeLibType(TypeLibTypeFlags.FCanCreate),
Guid(ShellCLSIDGuid.QueryParserManager)]
public class QueryParserManagerCoClass
{
}

# 声明 SearchFolderItemFactoryCoClass 类，作为搜索文件夹项工厂的 COM 类
[ComImport,
    ClassInterface(ClassInterfaceType.None),
TypeLibType(TypeLibTypeFlags.FCanCreate),
Guid(ShellCLSIDGuid.SearchFolderItemFactory)]
public class SearchFolderItemFactoryCoClass
{
}
```