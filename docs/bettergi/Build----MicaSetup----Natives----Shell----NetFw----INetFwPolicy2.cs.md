# `.\better-genshin-impact\Build\MicaSetup\Natives\Shell\NetFw\INetFwPolicy2.cs`

```cs
using System;
using System.Runtime.CompilerServices;
using System.Runtime.InteropServices;

namespace MicaSetup.Shell.NetFw;

// 定义一个接口 INetFwPolicy2，具有与 Windows 防火墙策略相关的属性和方法
[Guid("98325047-C671-4174-8D81-DEFCD3F03186"), TypeLibType(4160)]
[ComImport]
public interface INetFwPolicy2
{
    // 获取当前的配置文件类型
    [DispId(1)]
    int CurrentProfileTypes
    {
        [DispId(1)]
        [MethodImpl(MethodImplOptions.InternalCall)]
        get;
    }

    // 获取或设置防火墙是否启用
    [DispId(2)]
    bool FirewallEnabled
    {
        [DispId(2)]
        [MethodImpl(MethodImplOptions.InternalCall)]
        get;
        [DispId(2)]
        [MethodImpl(MethodImplOptions.InternalCall)]
        set;
    }

    // 获取或设置排除的接口
    [DispId(3)]
    object ExcludedInterfaces
    {
        [DispId(3)]
        [MethodImpl(MethodImplOptions.InternalCall)]
        [return: MarshalAs(UnmanagedType.Struct)]
        get;
        [DispId(3)]
        [MethodImpl(MethodImplOptions.InternalCall)]
        [param: MarshalAs(UnmanagedType.Struct)]
        set;
    }

    // 获取或设置是否阻止所有入站流量
    [DispId(4)]
    bool BlockAllInboundTraffic
    {
        [DispId(4)]
        [MethodImpl(MethodImplOptions.InternalCall)]
        get;
        [DispId(4)]
        [MethodImpl(MethodImplOptions.InternalCall)]
        set;
    }

    // 获取或设置是否禁用通知
    [DispId(5)]
    bool NotificationsDisabled
    {
        [DispId(5)]
        [MethodImpl(MethodImplOptions.InternalCall)]
        get;
        [DispId(5)]
        [MethodImpl(MethodImplOptions.InternalCall)]
        set;
    }

    // 获取或设置是否禁用单播响应多播广播
    [DispId(6)]
    bool UnicastResponsesToMulticastBroadcastDisabled
    {
        [DispId(6)]
        [MethodImpl(MethodImplOptions.InternalCall)]
        get;
        [DispId(6)]
        [MethodImpl(MethodImplOptions.InternalCall)]
        set;
    }

    // 获取与防火墙规则相关的 INetFwRules 接口
    [DispId(7)]
    INetFwRules Rules
    {
        [DispId(7)]
        [MethodImpl(MethodImplOptions.InternalCall)]
        [return: MarshalAs(UnmanagedType.Interface)]
        get;
    }

    // 获取与服务限制相关的 INetFwServiceRestriction 接口
    [DispId(8)]
    INetFwServiceRestriction ServiceRestriction
    {
        [DispId(8)]
        [MethodImpl(MethodImplOptions.InternalCall)]
        [return: MarshalAs(UnmanagedType.Interface)]
        get;
    }

    // 获取或设置默认的入站操作
    [DispId(12)]
    NET_FW_ACTION DefaultInboundAction
    {
        [DispId(12)]
        [MethodImpl(MethodImplOptions.InternalCall)]
        get;
        [DispId(12)]
        [MethodImpl(MethodImplOptions.InternalCall)]
        set;
    }

    // 获取或设置默认的出站操作
    [DispId(13)]
    NET_FW_ACTION DefaultOutboundAction
    {
        [DispId(13)]
        [MethodImpl(MethodImplOptions.InternalCall)]
        get;
        [DispId(13)]
        [MethodImpl(MethodImplOptions.InternalCall)]
        set;
    }

    // 获取当前规则组是否启用
    [DispId(14)]
    bool IsRuleGroupCurrentlyEnabled
    {
        [DispId(14)]
        [MethodImpl(MethodImplOptions.InternalCall)]
        get;
    }

    // 获取本地策略修改状态
    [DispId(15)]
    NET_FW_MODIFY_STATE LocalPolicyModifyState
    {
        [DispId(15)]
        [MethodImpl(MethodImplOptions.InternalCall)]
        get;
    }

    // 需要实现的接口方法
    [DispId(9)]
    [MethodImpl(MethodImplOptions.InternalCall)]
    // 声明一个方法，启用或禁用规则组，根据给定的配置文件类型位掩码、规则组名称和启用标志
    void EnableRuleGroup([In] int profileTypesBitmask, [MarshalAs(UnmanagedType.BStr)][In] string group, [In] bool enable);

    // 指定该方法的 ID 为 10，用于 COM 互操作
    [DispId(10)]
    // 指定方法实现方式为内部调用
    [MethodImpl(MethodImplOptions.InternalCall)]
    // 声明一个方法，检查给定的规则组是否已启用，返回布尔值
    bool IsRuleGroupEnabled([In] int profileTypesBitmask, [MarshalAs(UnmanagedType.BStr)][In] string group);

    // 指定该方法的 ID 为 11，用于 COM 互操作
    [DispId(11)]
    // 指定方法实现方式为内部调用
    [MethodImpl(MethodImplOptions.InternalCall)]
    // 声明一个方法，恢复本地防火墙的默认设置
    void RestoreLocalFirewallDefaults();
请提供需要注释的代码片段。
```