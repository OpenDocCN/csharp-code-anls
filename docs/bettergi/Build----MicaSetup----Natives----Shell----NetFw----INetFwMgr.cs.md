# `.\better-genshin-impact\Build\MicaSetup\Natives\Shell\NetFw\INetFwMgr.cs`

```cs
using System;
using System.Runtime.CompilerServices;
using System.Runtime.InteropServices;

namespace MicaSetup.Shell.NetFw;

# 声明一个 COM 接口 INetFwMgr，用于操作网络防火墙管理
[Guid("F7898AF5-CAC4-4632-A2EC-DA06E5111AF2"), TypeLibType(4160)]
[ComImport]
public interface INetFwMgr
{
    # 定义一个属性 LocalPolicy，用于获取本地防火墙策略
    [DispId(1)]
    INetFwPolicy LocalPolicy
    {
        [DispId(1)]
        [MethodImpl(MethodImplOptions.InternalCall)]
        [return: MarshalAs(UnmanagedType.Interface)]
        get; # 获取本地防火墙策略对象
    }

    # 定义一个属性 CurrentProfileType，用于获取当前配置文件类型
    [DispId(2)]
    NET_FW_PROFILE_TYPE CurrentProfileType
    {
        [DispId(2)]
        [MethodImpl(MethodImplOptions.InternalCall)]
        get; # 获取当前防火墙配置文件类型
    }

    # 定义一个方法 RestoreDefaults，用于恢复默认设置
    [DispId(3)]
    [MethodImpl(MethodImplOptions.InternalCall)]
    void RestoreDefaults(); # 恢复防火墙设置为默认值

    # 定义一个方法 IsPortAllowed，用于检查指定端口是否被允许
    [DispId(4)]
    [MethodImpl(MethodImplOptions.InternalCall)]
    void IsPortAllowed([MarshalAs(UnmanagedType.BStr)][In] string imageFileName, [In] NET_FW_IP_VERSION IpVersion, [In] int portNumber, [MarshalAs(UnmanagedType.BStr)][In] string localAddress, [In] NET_FW_IP_PROTOCOL ipProtocol, [MarshalAs(UnmanagedType.Struct)] out object allowed, [MarshalAs(UnmanagedType.Struct)] out object restricted);
    # 检查指定端口在给定 IP 版本、地址和协议下是否被允许或限制

    # 定义一个方法 IsIcmpTypeAllowed，用于检查 ICMP 类型是否被允许
    [DispId(5)]
    [MethodImpl(MethodImplOptions.InternalCall)]
    void IsIcmpTypeAllowed([In] NET_FW_IP_VERSION IpVersion, [MarshalAs(UnmanagedType.BStr)][In] string localAddress, [In] byte Type, [MarshalAs(UnmanagedType.Struct)] out object allowed, [MarshalAs(UnmanagedType.Struct)] out object restricted);
    # 检查指定 ICMP 类型在给定 IP 版本和地址下是否被允许或限制
}
```