# `.\better-genshin-impact\Build\MicaSetup\Natives\Shell\NetFw\INetFwOpenPorts.cs`

```cs
using System; // 引入 System 命名空间
using System.Collections; // 引入 System.Collections 命名空间，用于 IEnumerable 接口
using System.Runtime.CompilerServices; // 引入 System.Runtime.CompilerServices 命名空间，用于 MethodImpl 特性
using System.Runtime.InteropServices; // 引入 System.Runtime.InteropServices 命名空间，用于 COM 相关特性

namespace MicaSetup.Shell.NetFw; // 定义命名空间 MicaSetup.Shell.NetFw

#pragma warning disable CS0108 // 禁用 CS0108 警告，通常是由于成员隐藏导致的警告

[Guid("C0E9D7FA-E07E-430A-B19A-090CE82D92E2"), TypeLibType(4160)] // 定义接口的 GUID 和类型库类型
[ComImport] // 指示该接口是一个 COM 接口
public interface INetFwOpenPorts : IEnumerable // 定义 INetFwOpenPorts 接口，继承 IEnumerable 接口
{
    [DispId(1)] // 为 Count 属性指定 DispId 1
    int Count // Count 属性，用于获取开放端口的数量
    {
        [DispId(1)] // 为 Count 属性的 get 方法指定 DispId 1
        [MethodImpl(MethodImplOptions.InternalCall)] // 指定方法由内部调用实现
        get; // 仅有 getter
    }

    [DispId(2)] // 为 Add 方法指定 DispId 2
    [MethodImpl(MethodImplOptions.InternalCall)] // 指定方法由内部调用实现
    void Add([MarshalAs(UnmanagedType.Interface)][In] INetFwOpenPort Port); // 添加一个开放端口，参数为 INetFwOpenPort 类型

    [DispId(3)] // 为 Remove 方法指定 DispId 3
    [MethodImpl(MethodImplOptions.InternalCall)] // 指定方法由内部调用实现
    void Remove([In] int portNumber, [In] NET_FW_IP_PROTOCOL ipProtocol); // 移除指定的开放端口，参数为端口号和协议类型

    [DispId(4)] // 为 Item 方法指定 DispId 4
    [MethodImpl(MethodImplOptions.InternalCall)] // 指定方法由内部调用实现
    [return: MarshalAs(UnmanagedType.Interface)] // 指定返回值为 INetFwOpenPort 类型，并用 COM 接口进行封送
    INetFwOpenPort Item([In] int portNumber, [In] NET_FW_IP_PROTOCOL ipProtocol); // 获取指定端口号和协议类型的开放端口

    [DispId(-4), TypeLibFunc(1)] // 为 GetEnumerator 方法指定 DispId -4 和 TypeLibFunc 1
    [MethodImpl(MethodImplOptions.InternalCall)] // 指定方法由内部调用实现
    [return: MarshalAs(UnmanagedType.CustomMarshaler, MarshalType = "System.Runtime.InteropServices.CustomMarshalers.EnumeratorToEnumVariantMarshaler")] // 指定返回值的自定义封送器
    IEnumerator GetEnumerator(); // 获取用于枚举接口成员的枚举器
}
```