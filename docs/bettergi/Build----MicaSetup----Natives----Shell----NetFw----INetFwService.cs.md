# `.\better-genshin-impact\Build\MicaSetup\Natives\Shell\NetFw\INetFwService.cs`

```cs
using System;
using System.Runtime.CompilerServices;
using System.Runtime.InteropServices;

namespace MicaSetup.Shell.NetFw;

# 这个接口定义了与网络防火墙服务交互的方法和属性
[Guid("79FD57C8-908E-4A36-9888-D5B3F0A444CF"), TypeLibType(4160)]
[ComImport]
public interface INetFwService
{
    # 网络防火墙服务的名称属性
    [DispId(1)]
    string Name
    {
        # 获取网络防火墙服务的名称
        [DispId(1)]
        [MethodImpl(MethodImplOptions.InternalCall)]
        [return: MarshalAs(UnmanagedType.BStr)]
        get;
    }

    # 网络防火墙服务的类型属性
    [DispId(2)]
    NET_FW_SERVICE_TYPE Type
    {
        # 获取网络防火墙服务的类型
        [DispId(2)]
        [MethodImpl(MethodImplOptions.InternalCall)]
        get;
    }

    # 网络防火墙服务是否被自定义的属性
    [DispId(3)]
    bool Customized
    {
        # 获取网络防火墙服务是否被自定义
        [DispId(3)]
        [MethodImpl(MethodImplOptions.InternalCall)]
        get;
    }

    # 网络防火墙服务的 IP 版本属性
    [DispId(4)]
    NET_FW_IP_VERSION IpVersion
    {
        # 获取网络防火墙服务的 IP 版本
        [DispId(4)]
        [MethodImpl(MethodImplOptions.InternalCall)]
        get;
        # 设置网络防火墙服务的 IP 版本
        [DispId(4)]
        [MethodImpl(MethodImplOptions.InternalCall)]
        set;
    }

    # 网络防火墙服务的作用范围属性
    [DispId(5)]
    NET_FW_SCOPE Scope
    {
        # 获取网络防火墙服务的作用范围
        [DispId(5)]
        [MethodImpl(MethodImplOptions.InternalCall)]
        get;
        # 设置网络防火墙服务的作用范围
        [DispId(5)]
        [MethodImpl(MethodImplOptions.InternalCall)]
        set;
    }

    # 网络防火墙服务的远程地址属性
    [DispId(6)]
    string RemoteAddresses
    {
        # 获取网络防火墙服务的远程地址
        [DispId(6)]
        [MethodImpl(MethodImplOptions.InternalCall)]
        [return: MarshalAs(UnmanagedType.BStr)]
        get;
        # 设置网络防火墙服务的远程地址
        [DispId(6)]
        [MethodImpl(MethodImplOptions.InternalCall)]
        [param: MarshalAs(UnmanagedType.BStr)]
        set;
    }

    # 网络防火墙服务是否启用的属性
    [DispId(7)]
    bool Enabled
    {
        # 获取网络防火墙服务是否启用
        [DispId(7)]
        [MethodImpl(MethodImplOptions.InternalCall)]
        get;
        # 设置网络防火墙服务的启用状态
        [DispId(7)]
        [MethodImpl(MethodImplOptions.InternalCall)]
        set;
    }

    # 网络防火墙服务的全局开放端口属性
    [DispId(8)]
    INetFwOpenPorts GloballyOpenPorts
    {
        # 获取网络防火墙服务的全局开放端口
        [DispId(8)]
        [MethodImpl(MethodImplOptions.InternalCall)]
        [return: MarshalAs(UnmanagedType.Interface)]
        get;
    }
}
```