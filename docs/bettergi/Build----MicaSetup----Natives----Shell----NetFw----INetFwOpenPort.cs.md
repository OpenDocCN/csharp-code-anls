# `.\better-genshin-impact\Build\MicaSetup\Natives\Shell\NetFw\INetFwOpenPort.cs`

```cs
# 定义 COM 接口 INetFwOpenPort，提供有关网络防火墙开放端口的信息
[Guid("E0483BA0-47FF-4D9C-A6D6-7741D0B195F7"), TypeLibType(4160)]
[ComImport]
public interface INetFwOpenPort
{
    # 端口名称属性
    [DispId(1)]
    string Name
    {
        # 获取端口名称
        [DispId(1)]
        [MethodImpl(MethodImplOptions.InternalCall)]
        [return: MarshalAs(UnmanagedType.BStr)]
        get;
        # 设置端口名称
        [DispId(1)]
        [MethodImpl(MethodImplOptions.InternalCall)]
        [param: MarshalAs(UnmanagedType.BStr)]
        set;
    }

    # IP 版本属性
    [DispId(2)]
    NET_FW_IP_VERSION IpVersion
    {
        # 获取 IP 版本
        [DispId(2)]
        [MethodImpl(MethodImplOptions.InternalCall)]
        get;
        # 设置 IP 版本
        [DispId(2)]
        [MethodImpl(MethodImplOptions.InternalCall)]
        set;
    }

    # 协议类型属性
    [DispId(3)]
    NET_FW_IP_PROTOCOL Protocol
    {
        # 获取协议类型
        [DispId(3)]
        [MethodImpl(MethodImplOptions.InternalCall)]
        get;
        # 设置协议类型
        [DispId(3)]
        [MethodImpl(MethodImplOptions.InternalCall)]
        set;
    }

    # 端口号属性
    [DispId(4)]
    int Port
    {
        # 获取端口号
        [DispId(4)]
        [MethodImpl(MethodImplOptions.InternalCall)]
        get;
        # 设置端口号
        [DispId(4)]
        [MethodImpl(MethodImplOptions.InternalCall)]
        set;
    }

    # 作用域属性
    [DispId(5)]
    NET_FW_SCOPE Scope
    {
        # 获取作用域
        [DispId(5)]
        [MethodImpl(MethodImplOptions.InternalCall)]
        get;
        # 设置作用域
        [DispId(5)]
        [MethodImpl(MethodImplOptions.InternalCall)]
        set;
    }

    # 远程地址属性
    [DispId(6)]
    string RemoteAddresses
    {
        # 获取远程地址
        [DispId(6)]
        [MethodImpl(MethodImplOptions.InternalCall)]
        [return: MarshalAs(UnmanagedType.BStr)]
        get;
        # 设置远程地址
        [DispId(6)]
        [MethodImpl(MethodImplOptions.InternalCall)]
        [param: MarshalAs(UnmanagedType.BStr)]
        set;
    }

    # 是否启用属性
    [DispId(7)]
    bool Enabled
    {
        # 获取是否启用
        [DispId(7)]
        [MethodImpl(MethodImplOptions.InternalCall)]
        get;
        # 设置是否启用
        [DispId(7)]
        [MethodImpl(MethodImplOptions.InternalCall)]
        set;
    }

    # 是否为内置属性
    [DispId(8)]
    bool BuiltIn
    {
        # 获取是否为内置
        [DispId(8)]
        [MethodImpl(MethodImplOptions.InternalCall)]
        get;
    }
}
```