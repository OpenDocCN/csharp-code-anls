# `.\better-genshin-impact\Build\MicaSetup\Natives\Shell\NetFw\INetFwServiceRestriction.cs`

```cs
using System;  // 引入 System 命名空间，提供基本的系统功能和数据类型
using System.Runtime.CompilerServices;  // 引入 Runtime.CompilerServices 命名空间，用于编译器服务
using System.Runtime.InteropServices;  // 引入 Runtime.InteropServices 命名空间，用于 COM 互操作

namespace MicaSetup.Shell.NetFw;  // 定义命名空间 MicaSetup.Shell.NetFw

// 定义一个 COM 组件接口 INetFwServiceRestriction，使用指定的 GUID 和 TypeLibType
[Guid("8267BBE3-F890-491C-B7B6-2DB1EF0E5D2B"), TypeLibType(4160)]
[ComImport]  // 标记该接口为 COM 组件接口
public interface INetFwServiceRestriction
{
    // 定义一个属性 Rules，表示网络防火墙规则
    [DispId(3)]  // 指定该属性的 DISP ID 为 3
    INetFwRules Rules
    {
        [DispId(3)]  // 指定 get 访问器的 DISP ID 为 3
        [MethodImpl(MethodImplOptions.InternalCall)]  // 指示该方法由内部调用实现
        [return: MarshalAs(UnmanagedType.Interface)]  // 指定返回值应作为 COM 接口处理
        get;  // 获取 INetFwRules 接口
    }

    // 定义一个方法 RestrictService，用于限制服务的访问
    [DispId(1)]  // 指定该方法的 DISP ID 为 1
    [MethodImpl(MethodImplOptions.InternalCall)]  // 指示该方法由内部调用实现
    void RestrictService([MarshalAs(UnmanagedType.BStr)][In] string serviceName,  // 服务名称，作为 BStr 类型的字符串传入
                         [MarshalAs(UnmanagedType.BStr)][In] string appName,  // 应用名称，作为 BStr 类型的字符串传入
                         [In] bool RestrictService,  // 指定是否限制服务
                         [In] bool serviceSidRestricted);  // 指定服务 SID 是否受限

    // 定义一个方法 ServiceRestricted，用于检查服务是否受限
    [DispId(2)]  // 指定该方法的 DISP ID 为 2
    [MethodImpl(MethodImplOptions.InternalCall)]  // 指示该方法由内部调用实现
    bool ServiceRestricted([MarshalAs(UnmanagedType.BStr)][In] string serviceName,  // 服务名称，作为 BStr 类型的字符串传入
                           [MarshalAs(UnmanagedType.BStr)][In] string appName);  // 应用名称，作为 BStr 类型的字符串传入
}
```