# `.\better-genshin-impact\Build\MicaSetup\Natives\Shell\Dialogs\Interop\KnownFolders\KnownFoldersCOMInterfaces.cs`

```cs
using System;
using System.Runtime.CompilerServices;
using System.Runtime.InteropServices;

namespace MicaSetup.Shell.Dialogs;

#pragma warning disable CS0108
# 关闭对已隐藏成员的警告（CS0108）

[ComImport,
# 指示该接口由 COM 组件提供实现
Guid(KnownFoldersIIDGuid.IKnownFolderManager),
# 指定接口的 GUID 标识符
InterfaceType(ComInterfaceType.InterfaceIsIUnknown)]
# 指定接口的 COM 类型
internal interface IKnownFolderManager
{
    [MethodImpl(MethodImplOptions.InternalCall,
        MethodCodeType = MethodCodeType.Runtime)]
    # 指示该方法在运行时调用实现
    void FolderIdFromCsidl(int csidl,
       [Out] out Guid knownFolderID);
    # 根据 CSIDL 值获取对应的文件夹 GUID

    [MethodImpl(MethodImplOptions.InternalCall,
        MethodCodeType = MethodCodeType.Runtime)]
    void FolderIdToCsidl([In, MarshalAs(UnmanagedType.LPStruct)] Guid id,
      [Out] out int csidl);
    # 根据文件夹 GUID 获取对应的 CSIDL 值

    [MethodImpl(MethodImplOptions.InternalCall,
        MethodCodeType = MethodCodeType.Runtime)]
    void GetFolderIds([Out] out nint folders,
      [Out] out uint count);
    # 获取文件夹的 ID 列表以及数量

    [PreserveSig]
    # 指示在方法调用中不丢弃 HRESULT 错误代码
    [MethodImpl(MethodImplOptions.InternalCall,
        MethodCodeType = MethodCodeType.Runtime)]
    HResult GetFolder([In, MarshalAs(UnmanagedType.LPStruct)] Guid id,
      [Out, MarshalAs(UnmanagedType.Interface)] out IKnownFolderNative knownFolder);
    # 根据文件夹 GUID 获取对应的 IKnownFolderNative 实例

    [MethodImpl(MethodImplOptions.InternalCall,
        MethodCodeType = MethodCodeType.Runtime)]
    void GetFolderByName(string canonicalName,
      [Out, MarshalAs(UnmanagedType.Interface)] out IKnownFolderNative knownFolder);
    # 根据文件夹的标准名称获取对应的 IKnownFolderNative 实例

    [MethodImpl(MethodImplOptions.InternalCall,
        MethodCodeType = MethodCodeType.Runtime)]
    void RegisterFolder(
        [In, MarshalAs(UnmanagedType.LPStruct)] Guid knownFolderGuid,
        [In] ref KnownFoldersSafeNativeMethods.NativeFolderDefinition knownFolderDefinition);
    # 注册一个文件夹，指定 GUID 和文件夹定义

    [MethodImpl(MethodImplOptions.InternalCall,
        MethodCodeType = MethodCodeType.Runtime)]
    void UnregisterFolder(
        [In, MarshalAs(UnmanagedType.LPStruct)] Guid knownFolderGuid);
    # 注销一个已注册的文件夹，指定其 GUID

    [MethodImpl(MethodImplOptions.InternalCall, MethodCodeType = MethodCodeType.Runtime)]
    void FindFolderFromPath(
        [In, MarshalAs(UnmanagedType.LPWStr)] string path,
        [In] int mode,
        [Out, MarshalAs(UnmanagedType.Interface)] out IKnownFolderNative knownFolder);
    # 根据路径查找文件夹，返回对应的 IKnownFolderNative 实例

    [PreserveSig]
    # 指示在方法调用中不丢弃 HRESULT 错误代码
    [MethodImpl(MethodImplOptions.InternalCall,
        MethodCodeType = MethodCodeType.Runtime)]
    HResult FindFolderFromIDList(nint pidl, [Out, MarshalAs(UnmanagedType.Interface)] out IKnownFolderNative knownFolder);
    # 根据 PIDL 查找文件夹，返回对应的 IKnownFolderNative 实例

    [MethodImpl(MethodImplOptions.InternalCall,
        MethodCodeType = MethodCodeType.Runtime)]
    void Redirect();
    # 执行重定向操作
}

[ComImport,
    Guid(KnownFoldersIIDGuid.IKnownFolder),
# 指示该接口由 COM 组件提供实现
InterfaceType(ComInterfaceType.InterfaceIsIUnknown)]
# 指定接口的 COM 类型
internal interface IKnownFolderNative
{
    [MethodImpl(MethodImplOptions.InternalCall,
        MethodCodeType = MethodCodeType.Runtime)]
    Guid GetId();
    # 获取文件夹的 GUID

    [MethodImpl(MethodImplOptions.InternalCall,
        MethodCodeType = MethodCodeType.Runtime)]
    FolderCategory GetCategory();
    # 获取文件夹的类别

    [MethodImpl(MethodImplOptions.InternalCall,
        MethodCodeType = MethodCodeType.Runtime)]
    # 该方法用于获取 Shell 项目
    [PreserveSig]
    HResult GetShellItem([In] int i,
         ref Guid interfaceGuid,
         [Out, MarshalAs(UnmanagedType.Interface)] out IShellItem2 shellItem);
    
    # 该方法用于获取路径，并返回一个字符串类型
    [return: MarshalAs(UnmanagedType.LPWStr)]
    [MethodImpl(MethodImplOptions.InternalCall,
        MethodCodeType = MethodCodeType.Runtime)]
    string GetPath([In] int option);
    
    # 该方法用于设置路径
    [MethodImpl(MethodImplOptions.InternalCall,
        MethodCodeType = MethodCodeType.Runtime)]
    void SetPath([In] int i, [In] string path);
    
    # 该方法用于获取 ID 列表
    [MethodImpl(MethodImplOptions.InternalCall,
        MethodCodeType = MethodCodeType.Runtime)]
    void GetIDList([In] int i,
        [Out] out nint itemIdentifierListPointer);
    
    # 该方法用于获取文件夹类型的 GUID
    [MethodImpl(MethodImplOptions.InternalCall,
        MethodCodeType = MethodCodeType.Runtime)]
    Guid GetFolderType();
    
    # 该方法用于获取重定向能力
    [MethodImpl(MethodImplOptions.InternalCall,
        MethodCodeType = MethodCodeType.Runtime)]
    RedirectionCapability GetRedirectionCapabilities();
    
    # 该方法用于获取文件夹定义
    [MethodImpl(MethodImplOptions.InternalCall,
        MethodCodeType = MethodCodeType.Runtime)]
    void GetFolderDefinition(
        [Out, MarshalAs(UnmanagedType.Struct)] out KnownFoldersSafeNativeMethods.NativeFolderDefinition definition);
# 该类表示 KnownFolderManager 的 COM 实现
[ComImport]
[Guid("4df0c730-df9d-4ae3-9153-aa6b82e9795a")]
internal class KnownFolderManagerClass : IKnownFolderManager
{
    # 根据 CSIDL 值获取已知文件夹的 GUID
    [MethodImpl(MethodImplOptions.InternalCall,
        MethodCodeType = MethodCodeType.Runtime)]
    public extern virtual void FolderIdFromCsidl(int csidl,
        [Out] out Guid knownFolderID);

    # 根据已知文件夹的 GUID 获取 CSIDL 值
    [MethodImpl(MethodImplOptions.InternalCall,
        MethodCodeType = MethodCodeType.Runtime)]
    public extern virtual void FolderIdToCsidl(
        [In, MarshalAs(UnmanagedType.LPStruct)] Guid id,
        [Out] out int csidl);

    # 获取所有文件夹的 ID 列表
    [MethodImpl(MethodImplOptions.InternalCall,
        MethodCodeType = MethodCodeType.Runtime)]
    public extern virtual void GetFolderIds(
        [Out] out nint folders,
        [Out] out uint count);

    # 获取已知文件夹的详细信息
    [PreserveSig]
    [MethodImpl(MethodImplOptions.InternalCall,
        MethodCodeType = MethodCodeType.Runtime)]
    public extern virtual HResult GetFolder(
        [In, MarshalAs(UnmanagedType.LPStruct)] Guid id,
        [Out, MarshalAs(UnmanagedType.Interface)]
          out IKnownFolderNative knownFolder);

    # 根据文件夹的规范名称获取已知文件夹的详细信息
    [MethodImpl(MethodImplOptions.InternalCall,
        MethodCodeType = MethodCodeType.Runtime)]
    public extern virtual void GetFolderByName(
        string canonicalName,
        [Out, MarshalAs(UnmanagedType.Interface)] out IKnownFolderNative knownFolder);

    # 注册一个新的已知文件夹
    [MethodImpl(MethodImplOptions.InternalCall,
        MethodCodeType = MethodCodeType.Runtime)]
    public extern virtual void RegisterFolder(
        [In, MarshalAs(UnmanagedType.LPStruct)] Guid knownFolderGuid,
        [In] ref KnownFoldersSafeNativeMethods.NativeFolderDefinition knownFolderDefinition);

    # 注销一个已知文件夹
    [MethodImpl(MethodImplOptions.InternalCall,
        MethodCodeType = MethodCodeType.Runtime)]
    public extern virtual void UnregisterFolder(
        [In, MarshalAs(UnmanagedType.LPStruct)] Guid knownFolderGuid);

    # 从路径查找文件夹
    [MethodImpl(MethodImplOptions.InternalCall,
        MethodCodeType = MethodCodeType.Runtime)]
    public extern virtual void FindFolderFromPath(
        [In, MarshalAs(UnmanagedType.LPWStr)] string path,
        [In] int mode,
        [Out, MarshalAs(UnmanagedType.Interface)] out IKnownFolderNative knownFolder);

    # 从 ID 列表中查找文件夹
    [PreserveSig]
    [MethodImpl(MethodImplOptions.InternalCall,
        MethodCodeType = MethodCodeType.Runtime)]
    public extern virtual HResult FindFolderFromIDList(nint pidl, [Out, MarshalAs(UnmanagedType.Interface)] out IKnownFolderNative knownFolder);

    # 重定向到另一个位置
    [MethodImpl(MethodImplOptions.InternalCall,
        MethodCodeType = MethodCodeType.Runtime)]
    public extern virtual void Redirect();
}
```