# `.\better-genshin-impact\Build\MicaSetup\Natives\Shell\NetFw\INetFwRule2.cs`

```cs
using System;
using System.Runtime.CompilerServices;
using System.Runtime.InteropServices;

namespace MicaSetup.Shell.NetFw;

#pragma warning disable CS0108
#pragma warning disable IDE1006

// 定义一个 COM 接口，用于操作防火墙规则
[Guid("9C27C8DA-189B-4DDE-89F7-8B39A316782C"), TypeLibType(4160)]
[ComImport]
public interface INetFwRule2 : INetFwRule
{
    // 规则名称属性
    [DispId(1)]
    string Name
    {
        [DispId(1)]
        [MethodImpl(MethodImplOptions.InternalCall)]
        [return: MarshalAs(UnmanagedType.BStr)]
        get; // 获取规则名称
        [DispId(1)]
        [MethodImpl(MethodImplOptions.InternalCall)]
        [param: MarshalAs(UnmanagedType.BStr)]
        set; // 设置规则名称
    }

    // 规则描述属性
    [DispId(2)]
    string Description
    {
        [DispId(2)]
        [MethodImpl(MethodImplOptions.InternalCall)]
        [return: MarshalAs(UnmanagedType.BStr)]
        get; // 获取规则描述
        [DispId(2)]
        [MethodImpl(MethodImplOptions.InternalCall)]
        [param: MarshalAs(UnmanagedType.BStr)]
        set; // 设置规则描述
    }

    // 应用程序名称属性
    [DispId(3)]
    string ApplicationName
    {
        [DispId(3)]
        [MethodImpl(MethodImplOptions.InternalCall)]
        [return: MarshalAs(UnmanagedType.BStr)]
        get; // 获取应用程序名称
        [DispId(3)]
        [MethodImpl(MethodImplOptions.InternalCall)]
        [param: MarshalAs(UnmanagedType.BStr)]
        set; // 设置应用程序名称
    }

    // 服务名称属性
    [DispId(4)]
    string serviceName
    {
        [DispId(4)]
        [MethodImpl(MethodImplOptions.InternalCall)]
        [return: MarshalAs(UnmanagedType.BStr)]
        get; // 获取服务名称
        [DispId(4)]
        [MethodImpl(MethodImplOptions.InternalCall)]
        [param: MarshalAs(UnmanagedType.BStr)]
        set; // 设置服务名称
    }

    // 协议属性
    [DispId(5)]
    int Protocol
    {
        [DispId(5)]
        [MethodImpl(MethodImplOptions.InternalCall)]
        get; // 获取协议类型
        [DispId(5)]
        [MethodImpl(MethodImplOptions.InternalCall)]
        set; // 设置协议类型
    }

    // 本地端口属性
    [DispId(6)]
    string LocalPorts
    {
        [DispId(6)]
        [MethodImpl(MethodImplOptions.InternalCall)]
        [return: MarshalAs(UnmanagedType.BStr)]
        get; // 获取本地端口
        [DispId(6)]
        [MethodImpl(MethodImplOptions.InternalCall)]
        [param: MarshalAs(UnmanagedType.BStr)]
        set; // 设置本地端口
    }

    // 远程端口属性
    [DispId(7)]
    string RemotePorts
    {
        [DispId(7)]
        [MethodImpl(MethodImplOptions.InternalCall)]
        [return: MarshalAs(UnmanagedType.BStr)]
        get; // 获取远程端口
        [DispId(7)]
        [MethodImpl(MethodImplOptions.InternalCall)]
        [param: MarshalAs(UnmanagedType.BStr)]
        set; // 设置远程端口
    }

    // 本地地址属性
    [DispId(8)]
    string LocalAddresses
    {
        [DispId(8)]
        [MethodImpl(MethodImplOptions.InternalCall)]
        [return: MarshalAs(UnmanagedType.BStr)]
        get; // 获取本地地址
        [DispId(8)]
        [MethodImpl(MethodImplOptions.InternalCall)]
        [param: MarshalAs(UnmanagedType.BStr)]
        set; // 设置本地地址
    }

    // 远程地址属性
    [DispId(9)]
    string RemoteAddresses
    {
        # 表示属性的 getter 和 setter 方法的声明
        [DispId(9)]  # 属性的 DISP ID
        [MethodImpl(MethodImplOptions.InternalCall)]  # 表示方法是由外部调用的
        [return: MarshalAs(UnmanagedType.BStr)]  # 返回值类型为 BStr
        get;  # 读取属性值的 getter 方法
        [DispId(9)]  # 属性的 DISP ID
        [MethodImpl(MethodImplOptions.InternalCall)]  # 表示方法是由外部调用的
        [param: MarshalAs(UnmanagedType.BStr)]  # 参数类型为 BStr
        set;  # 设置属性值的 setter 方法
    }

    [DispId(10)]  # 属性的 DISP ID
    string IcmpTypesAndCodes  # 属性名和类型
    {
        # 属性的 getter 和 setter 方法的声明
        [DispId(10)]  # 属性的 DISP ID
        [MethodImpl(MethodImplOptions.InternalCall)]  # 表示方法是由外部调用的
        [return: MarshalAs(UnmanagedType.BStr)]  # 返回值类型为 BStr
        get;  # 读取属性值的 getter 方法
        [DispId(10)]  # 属性的 DISP ID
        [MethodImpl(MethodImplOptions.InternalCall)]  # 表示方法是由外部调用的
        [param: MarshalAs(UnmanagedType.BStr)]  # 参数类型为 BStr
        set;  # 设置属性值的 setter 方法
    }

    [DispId(11)]  # 属性的 DISP ID
    NET_FW_RULE_DIRECTION Direction  # 属性名和类型
    {
        # 属性的 getter 和 setter 方法的声明
        [DispId(11)]  # 属性的 DISP ID
        [MethodImpl(MethodImplOptions.InternalCall)]  # 表示方法是由外部调用的
        get;  # 读取属性值的 getter 方法
        [DispId(11)]  # 属性的 DISP ID
        [MethodImpl(MethodImplOptions.InternalCall)]  # 表示方法是由外部调用的
        set;  # 设置属性值的 setter 方法
    }

    [DispId(12)]  # 属性的 DISP ID
    object Interfaces  # 属性名和类型
    {
        # 属性的 getter 和 setter 方法的声明
        [DispId(12)]  # 属性的 DISP ID
        [MethodImpl(MethodImplOptions.InternalCall)]  # 表示方法是由外部调用的
        [return: MarshalAs(UnmanagedType.Struct)]  # 返回值类型为结构体
        get;  # 读取属性值的 getter 方法
        [DispId(12)]  # 属性的 DISP ID
        [MethodImpl(MethodImplOptions.InternalCall)]  # 表示方法是由外部调用的
        [param: MarshalAs(UnmanagedType.Struct)]  # 参数类型为结构体
        set;  # 设置属性值的 setter 方法
    }

    [DispId(13)]  # 属性的 DISP ID
    string InterfaceTypes  # 属性名和类型
    {
        # 属性的 getter 和 setter 方法的声明
        [DispId(13)]  # 属性的 DISP ID
        [MethodImpl(MethodImplOptions.InternalCall)]  # 表示方法是由外部调用的
        [return: MarshalAs(UnmanagedType.BStr)]  # 返回值类型为 BStr
        get;  # 读取属性值的 getter 方法
        [DispId(13)]  # 属性的 DISP ID
        [MethodImpl(MethodImplOptions.InternalCall)]  # 表示方法是由外部调用的
        [param: MarshalAs(UnmanagedType.BStr)]  # 参数类型为 BStr
        set;  # 设置属性值的 setter 方法
    }

    [DispId(14)]  # 属性的 DISP ID
    bool Enabled  # 属性名和类型
    {
        # 属性的 getter 和 setter 方法的声明
        [DispId(14)]  # 属性的 DISP ID
        [MethodImpl(MethodImplOptions.InternalCall)]  # 表示方法是由外部调用的
        get;  # 读取属性值的 getter 方法
        [DispId(14)]  # 属性的 DISP ID
        [MethodImpl(MethodImplOptions.InternalCall)]  # 表示方法是由外部调用的
        set;  # 设置属性值的 setter 方法
    }

    [DispId(15)]  # 属性的 DISP ID
    string Grouping  # 属性名和类型
    {
        # 属性的 getter 和 setter 方法的声明
        [DispId(15)]  # 属性的 DISP ID
        [MethodImpl(MethodImplOptions.InternalCall)]  # 表示方法是由外部调用的
        [return: MarshalAs(UnmanagedType.BStr)]  # 返回值类型为 BStr
        get;  # 读取属性值的 getter 方法
        [DispId(15)]  # 属性的 DISP ID
        [MethodImpl(MethodImplOptions.InternalCall)]  # 表示方法是由外部调用的
        [param: MarshalAs(UnmanagedType.BStr)]  # 参数类型为 BStr
        set;  # 设置属性值的 setter 方法
    }

    [DispId(16)]  # 属性的 DISP ID
    int Profiles  # 属性名和类型
    {
        # 属性的 getter 和 setter 方法的声明
        [DispId(16)]  # 属性的 DISP ID
        [MethodImpl(MethodImplOptions.InternalCall)]  # 表示方法是由外部调用的
        get;  # 读取属性值的 getter 方法
        [DispId(16)]  # 属性的 DISP ID
        [MethodImpl(MethodImplOptions.InternalCall)]  # 表示方法是由外部调用的
        set;  # 设置属性值的 setter 方法
    }

    [DispId(17)]  # 属性的 DISP ID
    bool EdgeTraversal  # 属性名和类型
    {
        # 属性的 getter 和 setter 方法的声明
        [DispId(17)]  # 属性的 DISP ID
        [MethodImpl(MethodImplOptions.InternalCall)]  # 表示方法是由外部调用的
        get;  # 读取属性值的 getter 方法
        [DispId(17)]  # 属性的 DISP ID
        [MethodImpl(MethodImplOptions.InternalCall)]  # 表示方法是由外部调用的
        set;  # 设置属性值的 setter 方法
    }

    [DispId(18)]  # 属性的 DISP ID
    NET_FW_ACTION Action  # 属性名和类型
    {
        # 属性的 getter 和 setter 方法的声明
        [DispId(18)]  # 属性的 DISP ID
        [MethodImpl(MethodImplOptions.InternalCall)]  # 表示方法是由外部调用的
        get;  # 读取属性值的 getter 方法
        [DispId(18)]  # 属性的 DISP ID
        [MethodImpl(MethodImplOptions.InternalCall)]  # 表示方法是由外部调用的
        set;  # 设置属性值的 setter 方法
    }

    [DispId(19)]  # 属性的 DISP ID
    int EdgeTraversalOptions  # 属性名和类型
    {
        # 属性的 getter 和 setter 方法的声明
        [DispId(19)]  # 属性的 DISP ID
        [MethodImpl(MethodImplOptions.InternalCall)]  # 表示方法是由外部调用的
        get;  # 读取属性值的 getter 方法
        [DispId(19)]  # 属性的 DISP ID
        [MethodImpl(MethodImplOptions.InternalCall)]  # 表示方法是由外部调用的
        set;  # 设置属性值的 setter 方法
    }
# 结束当前代码块（例如：一个函数、类、条件语句等）
}
```