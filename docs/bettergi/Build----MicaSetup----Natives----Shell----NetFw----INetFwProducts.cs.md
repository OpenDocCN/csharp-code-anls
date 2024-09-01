# `.\better-genshin-impact\Build\MicaSetup\Natives\Shell\NetFw\INetFwProducts.cs`

```cs
using System; // 引入 System 命名空间，提供基本的系统功能
using System.Collections; // 引入 System.Collections 命名空间，提供集合接口和类
using System.Runtime.CompilerServices; // 引入 System.Runtime.CompilerServices 命名空间，提供编译器相关的功能
using System.Runtime.InteropServices; // 引入 System.Runtime.InteropServices 命名空间，提供与 COM 互操作的功能

namespace MicaSetup.Shell.NetFw; // 定义一个名为 MicaSetup.Shell.NetFw 的命名空间

#pragma warning disable CS0108 // 禁用特定的编译器警告 CS0108（隐藏继承的成员）

[Guid("39EB36E0-2097-40BD-8AF2-63A13B525362"), TypeLibType(4160)] // 为接口指定 GUID 和类型库类型
[ComImport] // 指示该接口是从 COM 导入的
public interface INetFwProducts : IEnumerable // 定义一个名为 INetFwProducts 的接口，继承自 IEnumerable 接口
{
    [DispId(1)] // 为 Count 属性指定 DISP ID
    int Count // 定义一个名为 Count 的属性，表示产品数量
    {
        [DispId(1)] // 为 Count 属性的 get 访问器指定 DISP ID
        [MethodImpl(MethodImplOptions.InternalCall)] // 指定该方法是内部调用的
        get; // 获取产品数量
    }

    [DispId(2)] // 为 Register 方法指定 DISP ID
    [MethodImpl(MethodImplOptions.InternalCall)] // 指定该方法是内部调用的
    [return: MarshalAs(UnmanagedType.IUnknown)] // 指定返回类型为 IUnknown（COM 接口的通用基类型）
    object Register([MarshalAs(UnmanagedType.Interface)][In] INetFwProduct product); // 注册一个 INetFwProduct 对象

    [DispId(3)] // 为 Item 方法指定 DISP ID
    [MethodImpl(MethodImplOptions.InternalCall)] // 指定该方法是内部调用的
    [return: MarshalAs(UnmanagedType.Interface)] // 指定返回类型为 COM 接口
    INetFwProduct Item([In] int index); // 获取指定索引的 INetFwProduct 对象

    [DispId(-4), TypeLibFunc(1)] // 为 GetEnumerator 方法指定 DISP ID 和类型库函数标志
    [MethodImpl(MethodImplOptions.InternalCall)] // 指定该方法是内部调用的
    [return: MarshalAs(UnmanagedType.CustomMarshaler, MarshalType = "System.Runtime.InteropServices.CustomMarshalers.EnumeratorToEnumVariantMarshaler")] // 指定返回类型为自定义 marshaler 类型
    IEnumerator GetEnumerator(); // 获取用于枚举集合的 IEnumerator 对象
}
```