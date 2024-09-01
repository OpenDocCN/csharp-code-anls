# `.\better-genshin-impact\Build\MicaSetup\Natives\Shell\NetFw\INetFwProfile.cs`

```cs
using System; // 引入系统基础库
using System.Runtime.CompilerServices; // 引入用于方法实现的库
using System.Runtime.InteropServices; // 引入用于与 COM 互操作的库

namespace MicaSetup.Shell.NetFw; // 定义命名空间 MicaSetup.Shell.NetFw

// 为接口分配唯一的 GUID，指定接口类型库的类型
[Guid("174A0DDA-E9F9-449D-993B-21AB667CA456"), TypeLibType(4160)]
[ComImport] // 标记为 COM 导入接口
public interface INetFwProfile
{
    // 声明一个属性 Type，表示网络防火墙配置文件类型
    [DispId(1)] // 指定属性的 DISP ID
    NET_FW_PROFILE_TYPE Type
    {
        [DispId(1)] // 获取方法的 DISP ID
        [MethodImpl(MethodImplOptions.InternalCall)] // 标记为内部调用的方法
        get; // 获取属性值
    }

    // 声明一个属性 FirewallEnabled，表示防火墙是否启用
    [DispId(2)] // 指定属性的 DISP ID
    bool FirewallEnabled
    {
        [DispId(2)] // 获取方法的 DISP ID
        [MethodImpl(MethodImplOptions.InternalCall)] // 标记为内部调用的方法
        get; // 获取属性值
        [DispId(2)] // 设置方法的 DISP ID
        [MethodImpl(MethodImplOptions.InternalCall)] // 标记为内部调用的方法
        set; // 设置属性值
    }

    // 声明一个属性 ExceptionsNotAllowed，表示是否允许例外
    [DispId(3)] // 指定属性的 DISP ID
    bool ExceptionsNotAllowed
    {
        [DispId(3)] // 获取方法的 DISP ID
        [MethodImpl(MethodImplOptions.InternalCall)] // 标记为内部调用的方法
        get; // 获取属性值
        [DispId(3)] // 设置方法的 DISP ID
        [MethodImpl(MethodImplOptions.InternalCall)] // 标记为内部调用的方法
        set; // 设置属性值
    }

    // 声明一个属性 NotificationsDisabled，表示是否禁用通知
    [DispId(4)] // 指定属性的 DISP ID
    bool NotificationsDisabled
    {
        [DispId(4)] // 获取方法的 DISP ID
        [MethodImpl(MethodImplOptions.InternalCall)] // 标记为内部调用的方法
        get; // 获取属性值
        [DispId(4)] // 设置方法的 DISP ID
        [MethodImpl(MethodImplOptions.InternalCall)] // 标记为内部调用的方法
        set; // 设置属性值
    }

    // 声明一个属性 UnicastResponsesToMulticastBroadcastDisabled，表示是否禁用对多播广播的单播响应
    [DispId(5)] // 指定属性的 DISP ID
    bool UnicastResponsesToMulticastBroadcastDisabled
    {
        [DispId(5)] // 获取方法的 DISP ID
        [MethodImpl(MethodImplOptions.InternalCall)] // 标记为内部调用的方法
        get; // 获取属性值
        [DispId(5)] // 设置方法的 DISP ID
        [MethodImpl(MethodImplOptions.InternalCall)] // 标记为内部调用的方法
        set; // 设置属性值
    }

    // 声明一个属性 RemoteAdminSettings，表示远程管理设置
    [DispId(6)] // 指定属性的 DISP ID
    INetFwRemoteAdminSettings RemoteAdminSettings
    {
        [DispId(6)] // 获取方法的 DISP ID
        [MethodImpl(MethodImplOptions.InternalCall)] // 标记为内部调用的方法
        [return: MarshalAs(UnmanagedType.Interface)] // 指定返回值为 COM 接口
        get; // 获取属性值
    }

    // 声明一个属性 IcmpSettings，表示 ICMP 设置
    [DispId(7)] // 指定属性的 DISP ID
    INetFwIcmpSettings IcmpSettings
    {
        [DispId(7)] // 获取方法的 DISP ID
        [MethodImpl(MethodImplOptions.InternalCall)] // 标记为内部调用的方法
        [return: MarshalAs(UnmanagedType.Interface)] // 指定返回值为 COM 接口
        get; // 获取属性值
    }

    // 声明一个属性 GloballyOpenPorts，表示全局开放的端口
    [DispId(8)] // 指定属性的 DISP ID
    INetFwOpenPorts GloballyOpenPorts
    {
        [DispId(8)] // 获取方法的 DISP ID
        [MethodImpl(MethodImplOptions.InternalCall)] // 标记为内部调用的方法
        [return: MarshalAs(UnmanagedType.Interface)] // 指定返回值为 COM 接口
        get; // 获取属性值
    }

    // 声明一个属性 Services，表示服务设置
    [DispId(9)] // 指定属性的 DISP ID
    INetFwServices Services
    {
        [DispId(9)] // 获取方法的 DISP ID
        [MethodImpl(MethodImplOptions.InternalCall)] // 标记为内部调用的方法
        [return: MarshalAs(UnmanagedType.Interface)] // 指定返回值为 COM 接口
        get; // 获取属性值
    }

    // 声明一个属性 AuthorizedApplications，表示授权的应用程序
    [DispId(10)] // 指定属性的 DISP ID
    INetFwAuthorizedApplications AuthorizedApplications
    {
        [DispId(10)] // 获取方法的 DISP ID
        [MethodImpl(MethodImplOptions.InternalCall)] // 标记为内部调用的方法
        [return: MarshalAs(UnmanagedType.Interface)] // 指定返回值为 COM 接口
        get; // 获取属性值
    }
}
```