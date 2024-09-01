# `.\better-genshin-impact\Build\MicaSetup\Natives\Shell\NetFw\INetFwRemoteAdminSettings.cs`

```cs
using System; // 引入 System 命名空间
using System.Runtime.CompilerServices; // 引入用于方法实现的编译器服务命名空间
using System.Runtime.InteropServices; // 引入用于 COM 互操作的命名空间

namespace MicaSetup.Shell.NetFw; // 定义命名空间 MicaSetup.Shell.NetFw

// 定义 COM 接口 INetFwRemoteAdminSettings，接口 GUID 和 TypeLibType 属性指定了该接口的 COM 相关信息
[Guid("D4BECDDF-6F73-4A83-B832-9C66874CD20E"), TypeLibType(4160)]
[ComImport] // 指定该接口为 COM 接口
public interface INetFwRemoteAdminSettings
{
    // 属性 IpVersion，表示网络防火墙的 IP 版本
    [DispId(1)] // 指定该属性的 DISP ID 为 1
    NET_FW_IP_VERSION IpVersion
    {
        [DispId(1)] // 指定 getter 的 DISP ID 为 1
        [MethodImpl(MethodImplOptions.InternalCall)] // 表示方法实现由内部调用提供
        get; // 获取 IP 版本

        [DispId(1)] // 指定 setter 的 DISP ID 为 1
        [MethodImpl(MethodImplOptions.InternalCall)] // 表示方法实现由内部调用提供
        set; // 设置 IP 版本
    }

    // 属性 Scope，表示网络防火墙的作用范围
    [DispId(2)] // 指定该属性的 DISP ID 为 2
    NET_FW_SCOPE Scope
    {
        [DispId(2)] // 指定 getter 的 DISP ID 为 2
        [MethodImpl(MethodImplOptions.InternalCall)] // 表示方法实现由内部调用提供
        get; // 获取作用范围

        [DispId(2)] // 指定 setter 的 DISP ID 为 2
        [MethodImpl(MethodImplOptions.InternalCall)] // 表示方法实现由内部调用提供
        set; // 设置作用范围
    }

    // 属性 RemoteAddresses，表示远程地址的字符串
    [DispId(3)] // 指定该属性的 DISP ID 为 3
    string RemoteAddresses
    {
        [DispId(3)] // 指定 getter 的 DISP ID 为 3
        [MethodImpl(MethodImplOptions.InternalCall)] // 表示方法实现由内部调用提供
        [return: MarshalAs(UnmanagedType.BStr)] // 指定返回值类型为 BStr
        get; // 获取远程地址字符串

        [DispId(3)] // 指定 setter 的 DISP ID 为 3
        [MethodImpl(MethodImplOptions.InternalCall)] // 表示方法实现由内部调用提供
        [param: MarshalAs(UnmanagedType.BStr)] // 指定参数类型为 BStr
        set; // 设置远程地址字符串
    }

    // 属性 Enabled，表示是否启用
    [DispId(4)] // 指定该属性的 DISP ID 为 4
    bool Enabled
    {
        [DispId(4)] // 指定 getter 的 DISP ID 为 4
        [MethodImpl(MethodImplOptions.InternalCall)] // 表示方法实现由内部调用提供
        get; // 获取启用状态

        [DispId(4)] // 指定 setter 的 DISP ID 为 4
        [MethodImpl(MethodImplOptions.InternalCall)] // 表示方法实现由内部调用提供
        set; // 设置启用状态
    }
}
```