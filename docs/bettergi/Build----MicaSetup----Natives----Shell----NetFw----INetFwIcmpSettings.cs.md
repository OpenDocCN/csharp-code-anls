# `.\better-genshin-impact\Build\MicaSetup\Natives\Shell\NetFw\INetFwIcmpSettings.cs`

```cs
using System; // 引入 System 命名空间
using System.Runtime.CompilerServices; // 引入用于方法实现的命名空间
using System.Runtime.InteropServices; // 引入用于与 COM 交互的命名空间

namespace MicaSetup.Shell.NetFw; // 定义命名空间

// 指定 GUID 和 TypeLibType 特性，并标记此接口为 COM 导入
[Guid("A6207B2E-7CDD-426A-951E-5E1CBC5AFEAD"), TypeLibType(4160)]
[ComImport] // 指定此接口是从 COM 导入的
public interface INetFwIcmpSettings
{
    // 属性：允许传出目标不可达的 ICMP 数据包
    [DispId(1)]
    bool AllowOutboundDestinationUnreachable
    {
        // 获取方法
        [DispId(1)]
        [MethodImpl(MethodImplOptions.InternalCall)]
        get;
        // 设置方法
        [DispId(1)]
        [MethodImpl(MethodImplOptions.InternalCall)]
        set;
    }

    // 属性：允许 ICMP 重定向数据包
    [DispId(2)]
    bool AllowRedirect
    {
        // 获取方法
        [DispId(2)]
        [MethodImpl(MethodImplOptions.InternalCall)]
        get;
        // 设置方法
        [DispId(2)]
        [MethodImpl(MethodImplOptions.InternalCall)]
        set;
    }

    // 属性：允许传入的回显请求
    [DispId(3)]
    bool AllowInboundEchoRequest
    {
        // 获取方法
        [DispId(3)]
        [MethodImpl(MethodImplOptions.InternalCall)]
        get;
        // 设置方法
        [DispId(3)]
        [MethodImpl(MethodImplOptions.InternalCall)]
        set;
    }

    // 属性：允许传出时间超限数据包
    [DispId(4)]
    bool AllowOutboundTimeExceeded
    {
        // 获取方法
        [DispId(4)]
        [MethodImpl(MethodImplOptions.InternalCall)]
        get;
        // 设置方法
        [DispId(4)]
        [MethodImpl(MethodImplOptions.InternalCall)]
        set;
    }

    // 属性：允许传出参数问题数据包
    [DispId(5)]
    bool AllowOutboundParameterProblem
    {
        // 获取方法
        [DispId(5)]
        [MethodImpl(MethodImplOptions.InternalCall)]
        get;
        // 设置方法
        [DispId(5)]
        [MethodImpl(MethodImplOptions.InternalCall)]
        set;
    }

    // 属性：允许传出源抑制数据包
    [DispId(6)]
    bool AllowOutboundSourceQuench
    {
        // 获取方法
        [DispId(6)]
        [MethodImpl(MethodImplOptions.InternalCall)]
        get;
        // 设置方法
        [DispId(6)]
        [MethodImpl(MethodImplOptions.InternalCall)]
        set;
    }

    // 属性：允许传入路由器请求数据包
    [DispId(7)]
    bool AllowInboundRouterRequest
    {
        // 获取方法
        [DispId(7)]
        [MethodImpl(MethodImplOptions.InternalCall)]
        get;
        // 设置方法
        [DispId(7)]
        [MethodImpl(MethodImplOptions.InternalCall)]
        set;
    }

    // 属性：允许传入时间戳请求数据包
    [DispId(8)]
    bool AllowInboundTimestampRequest
    {
        // 获取方法
        [DispId(8)]
        [MethodImpl(MethodImplOptions.InternalCall)]
        get;
        // 设置方法
        [DispId(8)]
        [MethodImpl(MethodImplOptions.InternalCall)]
        set;
    }

    // 属性：允许传入掩码请求数据包
    [DispId(9)]
    bool AllowInboundMaskRequest
    {
        // 获取方法
        [DispId(9)]
        [MethodImpl(MethodImplOptions.InternalCall)]
        get;
        // 设置方法
        [DispId(9)]
        [MethodImpl(MethodImplOptions.InternalCall)]
        set;
    }

    // 属性：允许传出数据包过大错误
    [DispId(10)]
    bool AllowOutboundPacketTooBig
    {
        // 获取方法
        [DispId(10)]
        [MethodImpl(MethodImplOptions.InternalCall)]
        get;
        // 设置方法
        [DispId(10)]
        [MethodImpl(MethodImplOptions.InternalCall)]
        set;
    }
}
```