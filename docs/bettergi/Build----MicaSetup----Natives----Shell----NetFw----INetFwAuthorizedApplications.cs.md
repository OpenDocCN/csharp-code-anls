# `.\better-genshin-impact\Build\MicaSetup\Natives\Shell\NetFw\INetFwAuthorizedApplications.cs`

```cs
using System; // 引入 System 命名空间，提供基本的系统功能
using System.Collections; // 引入 System.Collections 命名空间，提供集合类的功能
using System.Runtime.CompilerServices; // 引入 System.Runtime.CompilerServices 命名空间，提供编译器相关的功能
using System.Runtime.InteropServices; // 引入 System.Runtime.InteropServices 命名空间，提供与 COM 互操作的功能

namespace MicaSetup.Shell.NetFw; // 定义 MicaSetup.Shell.NetFw 命名空间

#pragma warning disable CS0108 // 禁用特定的编译器警告 CS0108（成员隐藏基类成员）

[Guid("644EFD52-CCF9-486C-97A2-39F352570B30"), TypeLibType(4160)] // 指定接口的 GUID 和类型库类型
[ComImport] // 表示该接口由 COM 导入
public interface INetFwAuthorizedApplications : IEnumerable // 定义一个接口，继承 IEnumerable 接口
{
    [DispId(1)] // 指定接口成员的 DISP ID
    int Count // 定义一个只读属性，用于获取应用程序的数量
    {
        [DispId(1)] // 指定该属性的 DISP ID
        [MethodImpl(MethodImplOptions.InternalCall)] // 指示方法实现由内部调用
        get; // 定义属性的 getter
    }

    [DispId(2)] // 指定接口成员的 DISP ID
    [MethodImpl(MethodImplOptions.InternalCall)] // 指示方法实现由内部调用
    void Add([MarshalAs(UnmanagedType.Interface)][In] INetFwAuthorizedApplication app); // 定义一个方法，用于添加授权应用程序

    [DispId(3)] // 指定接口成员的 DISP ID
    [MethodImpl(MethodImplOptions.InternalCall)] // 指示方法实现由内部调用
    void Remove([MarshalAs(UnmanagedType.BStr)][In] string imageFileName); // 定义一个方法，用于移除指定的授权应用程序

    [DispId(4)] // 指定接口成员的 DISP ID
    [MethodImpl(MethodImplOptions.InternalCall)] // 指示方法实现由内部调用
    [return: MarshalAs(UnmanagedType.Interface)] // 指示返回值应作为 COM 对象处理
    INetFwAuthorizedApplication Item([MarshalAs(UnmanagedType.BStr)][In] string imageFileName); // 定义一个方法，用于获取指定名称的授权应用程序

    [DispId(-4), TypeLibFunc(1)] // 指定接口成员的 DISP ID 和类型库函数标志
    [MethodImpl(MethodImplOptions.InternalCall)] // 指示方法实现由内部调用
    [return: MarshalAs(UnmanagedType.CustomMarshaler, MarshalType = "System.Runtime.InteropServices.CustomMarshalers.EnumeratorToEnumVariantMarshaler")] // 指示返回值使用自定义的 Marshaler
    IEnumerator GetEnumerator(); // 定义一个方法，用于获取接口的枚举器
}
```