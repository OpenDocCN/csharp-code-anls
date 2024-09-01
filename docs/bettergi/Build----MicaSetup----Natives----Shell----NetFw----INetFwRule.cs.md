# `.\better-genshin-impact\Build\MicaSetup\Natives\Shell\NetFw\INetFwRule.cs`

```cs
using System;
using System.Runtime.CompilerServices;
using System.Runtime.InteropServices;

namespace MicaSetup.Shell.NetFw;

# 定义一个接口 INetFwRule 用于与 COM 组件进行交互
[Guid("AF230D27-BABA-4E42-ACED-F524F22CFCE2"), TypeLibType(4160)]
[ComImport]
public interface INetFwRule
{
    # 定义属性 Name，用于获取或设置规则的名称
    [DispId(1)]
    string Name
    {
        [DispId(1)]
        [MethodImpl(MethodImplOptions.InternalCall)]
        [return: MarshalAs(UnmanagedType.BStr)]
        get;
        [DispId(1)]
        [MethodImpl(MethodImplOptions.InternalCall)]
        [param: MarshalAs(UnmanagedType.BStr)]
        set;
    }

    # 定义属性 Description，用于获取或设置规则的描述
    [DispId(2)]
    string Description
    {
        [DispId(2)]
        [MethodImpl(MethodImplOptions.InternalCall)]
        [return: MarshalAs(UnmanagedType.BStr)]
        get;
        [DispId(2)]
        [MethodImpl(MethodImplOptions.InternalCall)]
        [param: MarshalAs(UnmanagedType.BStr)]
        set;
    }

    # 定义属性 ApplicationName，用于获取或设置规则关联的应用程序名称
    [DispId(3)]
    string ApplicationName
    {
        [DispId(3)]
        [MethodImpl(MethodImplOptions.InternalCall)]
        [return: MarshalAs(UnmanagedType.BStr)]
        get;
        [DispId(3)]
        [MethodImpl(MethodImplOptions.InternalCall)]
        [param: MarshalAs(UnmanagedType.BStr)]
        set;
    }

    # 定义属性 serviceName，用于获取或设置规则关联的服务名称
    [DispId(4)]
    string serviceName
    {
        [DispId(4)]
        [MethodImpl(MethodImplOptions.InternalCall)]
        [return: MarshalAs(UnmanagedType.BStr)]
        get;
        [DispId(4)]
        [MethodImpl(MethodImplOptions.InternalCall)]
        [param: MarshalAs(UnmanagedType.BStr)]
        set;
    }

    # 定义属性 Protocol，用于获取或设置协议类型
    [DispId(5)]
    int Protocol
    {
        [DispId(5)]
        [MethodImpl(MethodImplOptions.InternalCall)]
        get;
        [DispId(5)]
        [MethodImpl(MethodImplOptions.InternalCall)]
        set;
    }

    # 定义属性 LocalPorts，用于获取或设置本地端口
    [DispId(6)]
    string LocalPorts
    {
        [DispId(6)]
        [MethodImpl(MethodImplOptions.InternalCall)]
        [return: MarshalAs(UnmanagedType.BStr)]
        get;
        [DispId(6)]
        [MethodImpl(MethodImplOptions.InternalCall)]
        [param: MarshalAs(UnmanagedType.BStr)]
        set;
    }

    # 定义属性 RemotePorts，用于获取或设置远程端口
    [DispId(7)]
    string RemotePorts
    {
        [DispId(7)]
        [MethodImpl(MethodImplOptions.InternalCall)]
        [return: MarshalAs(UnmanagedType.BStr)]
        get;
        [DispId(7)]
        [MethodImpl(MethodImplOptions.InternalCall)]
        [param: MarshalAs(UnmanagedType.BStr)]
        set;
    }

    # 定义属性 LocalAddresses，用于获取或设置本地地址
    [DispId(8)]
    string LocalAddresses
    {
        [DispId(8)]
        [MethodImpl(MethodImplOptions.InternalCall)]
        [return: MarshalAs(UnmanagedType.BStr)]
        get;
        [DispId(8)]
        [MethodImpl(MethodImplOptions.InternalCall)]
        [param: MarshalAs(UnmanagedType.BStr)]
        set;
    }

    # 定义属性 RemoteAddresses，用于获取或设置远程地址
    [DispId(9)]
    string RemoteAddresses
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

    # 指定接口成员的 DispId 为 10，并提供字符串属性 IcmpTypesAndCodes 的访问器
    [DispId(10)]
    string IcmpTypesAndCodes
    {
        # 指定接口成员的 DispId 为 10，声明访问器方法为内部调用，并返回 BStr 类型
        [DispId(10)]
        [MethodImpl(MethodImplOptions.InternalCall)]
        [return: MarshalAs(UnmanagedType.BStr)]
        get;
        # 指定接口成员的 DispId 为 10，声明访问器方法为内部调用，并设置参数为 BStr 类型
        [DispId(10)]
        [MethodImpl(MethodImplOptions.InternalCall)]
        [param: MarshalAs(UnmanagedType.BStr)]
        set;
    }

    # 指定接口成员的 DispId 为 11，并提供 NET_FW_RULE_DIRECTION 枚举属性 Direction 的访问器
    [DispId(11)]
    NET_FW_RULE_DIRECTION Direction
    {
        # 指定接口成员的 DispId 为 11，声明访问器方法为内部调用
        [DispId(11)]
        [MethodImpl(MethodImplOptions.InternalCall)]
        get;
        # 指定接口成员的 DispId 为 11，声明访问器方法为内部调用
        [DispId(11)]
        [MethodImpl(MethodImplOptions.InternalCall)]
        set;
    }

    # 指定接口成员的 DispId 为 12，并提供 object 类型属性 Interfaces 的访问器
    [DispId(12)]
    object Interfaces
    {
        # 指定接口成员的 DispId 为 12，声明访问器方法为内部调用，并返回结构体类型
        [DispId(12)]
        [MethodImpl(MethodImplOptions.InternalCall)]
        [return: MarshalAs(UnmanagedType.Struct)]
        get;
        # 指定接口成员的 DispId 为 12，声明访问器方法为内部调用，并设置参数为结构体类型
        [DispId(12)]
        [MethodImpl(MethodImplOptions.InternalCall)]
        [param: MarshalAs(UnmanagedType.Struct)]
        set;
    }

    # 指定接口成员的 DispId 为 13，并提供字符串属性 InterfaceTypes 的访问器
    [DispId(13)]
    string InterfaceTypes
    {
        # 指定接口成员的 DispId 为 13，声明访问器方法为内部调用，并返回 BStr 类型
        [DispId(13)]
        [MethodImpl(MethodImplOptions.InternalCall)]
        [return: MarshalAs(UnmanagedType.BStr)]
        get;
        # 指定接口成员的 DispId 为 13，声明访问器方法为内部调用，并设置参数为 BStr 类型
        [DispId(13)]
        [MethodImpl(MethodImplOptions.InternalCall)]
        [param: MarshalAs(UnmanagedType.BStr)]
        set;
    }

    # 指定接口成员的 DispId 为 14，并提供布尔值属性 Enabled 的访问器
    [DispId(14)]
    bool Enabled
    {
        # 指定接口成员的 DispId 为 14，声明访问器方法为内部调用
        [DispId(14)]
        [MethodImpl(MethodImplOptions.InternalCall)]
        get;
        # 指定接口成员的 DispId 为 14，声明访问器方法为内部调用
        [DispId(14)]
        [MethodImpl(MethodImplOptions.InternalCall)]
        set;
    }

    # 指定接口成员的 DispId 为 15，并提供字符串属性 Grouping 的访问器
    [DispId(15)]
    string Grouping
    {
        # 指定接口成员的 DispId 为 15，声明访问器方法为内部调用，并返回 BStr 类型
        [DispId(15)]
        [MethodImpl(MethodImplOptions.InternalCall)]
        [return: MarshalAs(UnmanagedType.BStr)]
        get;
        # 指定接口成员的 DispId 为 15，声明访问器方法为内部调用，并设置参数为 BStr 类型
        [DispId(15)]
        [MethodImpl(MethodImplOptions.InternalCall)]
        [param: MarshalAs(UnmanagedType.BStr)]
        set;
    }

    # 指定接口成员的 DispId 为 16，并提供整数属性 Profiles 的访问器
    [DispId(16)]
    int Profiles
    {
        # 指定接口成员的 DispId 为 16，声明访问器方法为内部调用
        [DispId(16)]
        [MethodImpl(MethodImplOptions.InternalCall)]
        get;
        # 指定接口成员的 DispId 为 16，声明访问器方法为内部调用
        [DispId(16)]
        [MethodImpl(MethodImplOptions.InternalCall)]
        set;
    }

    # 指定接口成员的 DispId 为 17，并提供布尔值属性 EdgeTraversal 的访问器
    [DispId(17)]
    bool EdgeTraversal
    {
        # 指定接口成员的 DispId 为 17，声明访问器方法为内部调用
        [DispId(17)]
        [MethodImpl(MethodImplOptions.InternalCall)]
        get;
        # 指定接口成员的 DispId 为 17，声明访问器方法为内部调用
        [DispId(17)]
        [MethodImpl(MethodImplOptions.InternalCall)]
        set;
    }

    # 指定接口成员的 DispId 为 18，并提供 NET_FW_ACTION 枚举属性 Action 的访问器
    [DispId(18)]
    NET_FW_ACTION Action
    {
        # 指定接口成员的 DispId 为 18，声明访问器方法为内部调用
        [DispId(18)]
        [MethodImpl(MethodImplOptions.InternalCall)]
        get;
        # 指定接口成员的 DispId 为 18，声明访问器方法为内部调用
        [DispId(18)]
        [MethodImpl(MethodImplOptions.InternalCall)]
        set;
    }
能否提供更多代码上下文？这样可以更准确地解释每一行的作用。
```