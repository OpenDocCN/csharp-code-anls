# `.\better-genshin-impact\Build\MicaSetup\Natives\Shell\NetFw\INetFwRule3.cs`

```cs
using System;
using System.Runtime.CompilerServices;
using System.Runtime.InteropServices;

namespace MicaSetup.Shell.NetFw;

#pragma warning disable CS0108
#pragma warning disable IDE1006

# 定义一个 COM 接口 INetFwRule3，并继承自 INetFwRule2
[Guid("B21563FF-D696-4222-AB46-4E89B73AB34A"), TypeLibType(4160)]
[ComImport]
public interface INetFwRule3 : INetFwRule2
{
    # 规则名称属性
    [DispId(1)]
    string Name
    {
        # 获取规则名称
        [DispId(1)]
        [MethodImpl(MethodImplOptions.InternalCall)]
        [return: MarshalAs(UnmanagedType.BStr)]
        get;
        # 设置规则名称
        [DispId(1)]
        [MethodImpl(MethodImplOptions.InternalCall)]
        [param: MarshalAs(UnmanagedType.BStr)]
        set;
    }

    # 规则描述属性
    [DispId(2)]
    string Description
    {
        # 获取规则描述
        [DispId(2)]
        [MethodImpl(MethodImplOptions.InternalCall)]
        [return: MarshalAs(UnmanagedType.BStr)]
        get;
        # 设置规则描述
        [DispId(2)]
        [MethodImpl(MethodImplOptions.InternalCall)]
        [param: MarshalAs(UnmanagedType.BStr)]
        set;
    }

    # 应用程序名称属性
    [DispId(3)]
    string ApplicationName
    {
        # 获取应用程序名称
        [DispId(3)]
        [MethodImpl(MethodImplOptions.InternalCall)]
        [return: MarshalAs(UnmanagedType.BStr)]
        get;
        # 设置应用程序名称
        [DispId(3)]
        [MethodImpl(MethodImplOptions.InternalCall)]
        [param: MarshalAs(UnmanagedType.BStr)]
        set;
    }

    # 服务名称属性
    [DispId(4)]
    string serviceName
    {
        # 获取服务名称
        [DispId(4)]
        [MethodImpl(MethodImplOptions.InternalCall)]
        [return: MarshalAs(UnmanagedType.BStr)]
        get;
        # 设置服务名称
        [DispId(4)]
        [MethodImpl(MethodImplOptions.InternalCall)]
        [param: MarshalAs(UnmanagedType.BStr)]
        set;
    }

    # 协议属性
    [DispId(5)]
    int Protocol
    {
        # 获取协议类型
        [DispId(5)]
        [MethodImpl(MethodImplOptions.InternalCall)]
        get;
        # 设置协议类型
        [DispId(5)]
        [MethodImpl(MethodImplOptions.InternalCall)]
        set;
    }

    # 本地端口属性
    [DispId(6)]
    string LocalPorts
    {
        # 获取本地端口
        [DispId(6)]
        [MethodImpl(MethodImplOptions.InternalCall)]
        [return: MarshalAs(UnmanagedType.BStr)]
        get;
        # 设置本地端口
        [DispId(6)]
        [MethodImpl(MethodImplOptions.InternalCall)]
        [param: MarshalAs(UnmanagedType.BStr)]
        set;
    }

    # 远程端口属性
    [DispId(7)]
    string RemotePorts
    {
        # 获取远程端口
        [DispId(7)]
        [MethodImpl(MethodImplOptions.InternalCall)]
        [return: MarshalAs(UnmanagedType.BStr)]
        get;
        # 设置远程端口
        [DispId(7)]
        [MethodImpl(MethodImplOptions.InternalCall)]
        [param: MarshalAs(UnmanagedType.BStr)]
        set;
    }

    # 本地地址属性
    [DispId(8)]
    string LocalAddresses
    {
        # 获取本地地址
        [DispId(8)]
        [MethodImpl(MethodImplOptions.InternalCall)]
        [return: MarshalAs(UnmanagedType.BStr)]
        get;
        # 设置本地地址
        [DispId(8)]
        [MethodImpl(MethodImplOptions.InternalCall)]
        [param: MarshalAs(UnmanagedType.BStr)]
        set;
    }

    # 远程地址属性
    [DispId(9)]
    string RemoteAddresses
    # 定义属性，DispId 为 9，表示获取和设置一个 BStr 类型的属性
    {
        [DispId(9)]
        [MethodImpl(MethodImplOptions.InternalCall)]
        [return: MarshalAs(UnmanagedType.BStr)]
        get;
        [DispId(9)]
        [MethodImpl(MethodImplOptions.InternalCall)]
        [param: MarshalAs(UnmanagedType.BStr)]
        set;
    }

    # 定义属性，DispId 为 10，表示获取和设置一个 BStr 类型的属性
    [DispId(10)]
    string IcmpTypesAndCodes
    {
        [DispId(10)]
        [MethodImpl(MethodImplOptions.InternalCall)]
        [return: MarshalAs(UnmanagedType.BStr)]
        get;
        [DispId(10)]
        [MethodImpl(MethodImplOptions.InternalCall)]
        [param: MarshalAs(UnmanagedType.BStr)]
        set;
    }

    # 定义属性，DispId 为 11，表示获取和设置 NET_FW_RULE_DIRECTION 类型的属性
    [DispId(11)]
    NET_FW_RULE_DIRECTION Direction
    {
        [DispId(11)]
        [MethodImpl(MethodImplOptions.InternalCall)]
        get;
        [DispId(11)]
        [MethodImpl(MethodImplOptions.InternalCall)]
        set;
    }

    # 定义属性，DispId 为 12，表示获取和设置一个结构体类型的属性
    [DispId(12)]
    object Interfaces
    {
        [DispId(12)]
        [MethodImpl(MethodImplOptions.InternalCall)]
        [return: MarshalAs(UnmanagedType.Struct)]
        get;
        [DispId(12)]
        [MethodImpl(MethodImplOptions.InternalCall)]
        [param: MarshalAs(UnmanagedType.Struct)]
        set;
    }

    # 定义属性，DispId 为 13，表示获取和设置一个 BStr 类型的属性
    [DispId(13)]
    string InterfaceTypes
    {
        [DispId(13)]
        [MethodImpl(MethodImplOptions.InternalCall)]
        [return: MarshalAs(UnmanagedType.BStr)]
        get;
        [DispId(13)]
        [MethodImpl(MethodImplOptions.InternalCall)]
        [param: MarshalAs(UnmanagedType.BStr)]
        set;
    }

    # 定义属性，DispId 为 14，表示获取和设置一个布尔值类型的属性
    [DispId(14)]
    bool Enabled
    {
        [DispId(14)]
        [MethodImpl(MethodImplOptions.InternalCall)]
        get;
        [DispId(14)]
        [MethodImpl(MethodImplOptions.InternalCall)]
        set;
    }

    # 定义属性，DispId 为 15，表示获取和设置一个 BStr 类型的属性
    [DispId(15)]
    string Grouping
    {
        [DispId(15)]
        [MethodImpl(MethodImplOptions.InternalCall)]
        [return: MarshalAs(UnmanagedType.BStr)]
        get;
        [DispId(15)]
        [MethodImpl(MethodImplOptions.InternalCall)]
        [param: MarshalAs(UnmanagedType.BStr)]
        set;
    }

    # 定义属性，DispId 为 16，表示获取和设置一个整型属性
    [DispId(16)]
    int Profiles
    {
        [DispId(16)]
        [MethodImpl(MethodImplOptions.InternalCall)]
        get;
        [DispId(16)]
        [MethodImpl(MethodImplOptions.InternalCall)]
        set;
    }

    # 定义属性，DispId 为 17，表示获取和设置一个布尔值类型的属性
    [DispId(17)]
    bool EdgeTraversal
    {
        [DispId(17)]
        [MethodImpl(MethodImplOptions.InternalCall)]
        get;
        [DispId(17)]
        [MethodImpl(MethodImplOptions.InternalCall)]
        set;
    }

    # 定义属性，DispId 为 18，表示获取和设置 NET_FW_ACTION 类型的属性
    [DispId(18)]
    NET_FW_ACTION Action
    {
        [DispId(18)]
        [MethodImpl(MethodImplOptions.InternalCall)]
        get;
        [DispId(18)]
        [MethodImpl(MethodImplOptions.InternalCall)]
        set;
    }

    # 定义属性，DispId 为 19，表示获取和设置一个整型属性
    [DispId(19)]
    int EdgeTraversalOptions
    {
        [DispId(19)]
        [MethodImpl(MethodImplOptions.InternalCall)]
        get;
        [DispId(19)]
        [MethodImpl(MethodImplOptions.InternalCall)]
        set;
    }

    # 定义属性，DispId 为 20，表示获取和设置一个 BStr 类型的属性
    [DispId(20)]
    string LocalAppPackageId
    # 属性定义，用于表示某个数据的访问器和修改器
    {
        # 该属性的 ID 为 20，标识符
        [DispId(20)]
        # 方法实现选项为 InternalCall，表示调用的是内部方法
        [MethodImpl(MethodImplOptions.InternalCall)]
        # 该属性的 getter 方法返回值将被转换为 BStr 类型
        [return: MarshalAs(UnmanagedType.BStr)]
        get;
        # 该属性的 ID 为 20，标识符
        [DispId(20)]
        # 方法实现选项为 InternalCall，表示调用的是内部方法
        [MethodImpl(MethodImplOptions.InternalCall)]
        # 该属性的 setter 方法参数将被转换为 BStr 类型
        [param: MarshalAs(UnmanagedType.BStr)]
        set;
    }

    # 属性定义，用于表示本地用户拥有者的访问器和修改器
    [DispId(21)]
    string LocalUserOwner
    {
        # 该属性的 ID 为 21，标识符
        [DispId(21)]
        # 方法实现选项为 InternalCall，表示调用的是内部方法
        [MethodImpl(MethodImplOptions.InternalCall)]
        # 该属性的 getter 方法返回值将被转换为 BStr 类型
        [return: MarshalAs(UnmanagedType.BStr)]
        get;
        # 该属性的 ID 为 21，标识符
        [DispId(21)]
        # 方法实现选项为 InternalCall，表示调用的是内部方法
        [MethodImpl(MethodImplOptions.InternalCall)]
        # 该属性的 setter 方法参数将被转换为 BStr 类型
        [param: MarshalAs(UnmanagedType.BStr)]
        set;
    }

    # 属性定义，用于表示本地用户授权列表的访问器和修改器
    [DispId(22)]
    string LocalUserAuthorizedList
    {
        # 该属性的 ID 为 22，标识符
        [DispId(22)]
        # 方法实现选项为 InternalCall，表示调用的是内部方法
        [MethodImpl(MethodImplOptions.InternalCall)]
        # 该属性的 getter 方法返回值将被转换为 BStr 类型
        [return: MarshalAs(UnmanagedType.BStr)]
        get;
        # 该属性的 ID 为 22，标识符
        [DispId(22)]
        # 方法实现选项为 InternalCall，表示调用的是内部方法
        [MethodImpl(MethodImplOptions.InternalCall)]
        # 该属性的 setter 方法参数将被转换为 BStr 类型
        [param: MarshalAs(UnmanagedType.BStr)]
        set;
    }

    # 属性定义，用于表示远程用户授权列表的访问器和修改器
    [DispId(23)]
    string RemoteUserAuthorizedList
    {
        # 该属性的 ID 为 23，标识符
        [DispId(23)]
        # 方法实现选项为 InternalCall，表示调用的是内部方法
        [MethodImpl(MethodImplOptions.InternalCall)]
        # 该属性的 getter 方法返回值将被转换为 BStr 类型
        [return: MarshalAs(UnmanagedType.BStr)]
        get;
        # 该属性的 ID 为 23，标识符
        [DispId(23)]
        # 方法实现选项为 InternalCall，表示调用的是内部方法
        [MethodImpl(MethodImplOptions.InternalCall)]
        # 该属性的 setter 方法参数将被转换为 BStr 类型
        [param: MarshalAs(UnmanagedType.BStr)]
        set;
    }

    # 属性定义，用于表示远程机器授权列表的访问器和修改器
    [DispId(24)]
    string RemoteMachineAuthorizedList
    {
        # 该属性的 ID 为 24，标识符
        [DispId(24)]
        # 方法实现选项为 InternalCall，表示调用的是内部方法
        [MethodImpl(MethodImplOptions.InternalCall)]
        # 该属性的 getter 方法返回值将被转换为 BStr 类型
        [return: MarshalAs(UnmanagedType.BStr)]
        get;
        # 该属性的 ID 为 24，标识符
        [DispId(24)]
        # 方法实现选项为 InternalCall，表示调用的是内部方法
        [MethodImpl(MethodImplOptions.InternalCall)]
        # 该属性的 setter 方法参数将被转换为 BStr 类型
        [param: MarshalAs(UnmanagedType.BStr)]
        set;
    }

    # 属性定义，用于表示安全标志的访问器和修改器
    [DispId(25)]
    int SecureFlags
    {
        # 该属性的 ID 为 25，标识符
        [DispId(25)]
        # 方法实现选项为 InternalCall，表示调用的是内部方法
        [MethodImpl(MethodImplOptions.InternalCall)]
        # 该属性的 getter 方法返回值为 int 类型
        get;
        # 该属性的 ID 为 25，标识符
        [DispId(25)]
        # 方法实现选项为 InternalCall，表示调用的是内部方法
        [MethodImpl(MethodImplOptions.InternalCall)]
        # 该属性的 setter 方法参数为 int 类型
        set;
    }
# 结束当前代码块或代码段的右大括号
}
```