# `.\better-genshin-impact\Build\MicaSetup\Natives\Shell\NetFw\INetFwAuthorizedApplication.cs`

```cs
using System;  // 引入系统命名空间，提供基本的系统功能
using System.Runtime.CompilerServices;  // 引入编译器服务命名空间，提供方法实现和编译器指令
using System.Runtime.InteropServices;  // 引入互操作服务命名空间，提供与 COM 组件的互操作功能

namespace MicaSetup.Shell.NetFw;  // 定义命名空间 MicaSetup.Shell.NetFw，用于组织代码

// 定义一个 COM 组件接口 INetFwAuthorizedApplication，指定 GUID 和 TypeLibType
[Guid("B5E64FFA-C2C5-444E-A301-FB5E00018050"), TypeLibType(4160)]
[ComImport]  // 标记此接口为 COM 导入接口
public interface INetFwAuthorizedApplication
{
    // 定义 Name 属性，具有 get 和 set 方法
    [DispId(1)]  // 指定属性的 DISP ID
    string Name
    {
        [DispId(1)]  // 指定 get 方法的 DISP ID
        [MethodImpl(MethodImplOptions.InternalCall)]  // 指示方法实现由 COM 提供
        [return: MarshalAs(UnmanagedType.BStr)]  // 指定返回值类型为 BStr
        get;  // 属性的 getter 方法
        [DispId(1)]  // 指定 set 方法的 DISP ID
        [MethodImpl(MethodImplOptions.InternalCall)]  // 指示方法实现由 COM 提供
        [param: MarshalAs(UnmanagedType.BStr)]  // 指定参数类型为 BStr
        set;  // 属性的 setter 方法
    }

    // 定义 ProcessImageFileName 属性，具有 get 和 set 方法
    [DispId(2)]  // 指定属性的 DISP ID
    string ProcessImageFileName
    {
        [DispId(2)]  // 指定 get 方法的 DISP ID
        [MethodImpl(MethodImplOptions.InternalCall)]  // 指示方法实现由 COM 提供
        [return: MarshalAs(UnmanagedType.BStr)]  // 指定返回值类型为 BStr
        get;  // 属性的 getter 方法
        [DispId(2)]  // 指定 set 方法的 DISP ID
        [MethodImpl(MethodImplOptions.InternalCall)]  // 指示方法实现由 COM 提供
        [param: MarshalAs(UnmanagedType.BStr)]  // 指定参数类型为 BStr
        set;  // 属性的 setter 方法
    }

    // 定义 IpVersion 属性，具有 get 和 set 方法
    [DispId(3)]  // 指定属性的 DISP ID
    NET_FW_IP_VERSION IpVersion
    {
        [DispId(3)]  // 指定 get 方法的 DISP ID
        [MethodImpl(MethodImplOptions.InternalCall)]  // 指示方法实现由 COM 提供
        get;  // 属性的 getter 方法
        [DispId(3)]  // 指定 set 方法的 DISP ID
        [MethodImpl(MethodImplOptions.InternalCall)]  // 指示方法实现由 COM 提供
        set;  // 属性的 setter 方法
    }

    // 定义 Scope 属性，具有 get 和 set 方法
    [DispId(4)]  // 指定属性的 DISP ID
    NET_FW_SCOPE Scope
    {
        [DispId(4)]  // 指定 get 方法的 DISP ID
        [MethodImpl(MethodImplOptions.InternalCall)]  // 指示方法实现由 COM 提供
        get;  // 属性的 getter 方法
        [DispId(4)]  // 指定 set 方法的 DISP ID
        [MethodImpl(MethodImplOptions.InternalCall)]  // 指示方法实现由 COM 提供
        set;  // 属性的 setter 方法
    }

    // 定义 RemoteAddresses 属性，具有 get 和 set 方法
    [DispId(5)]  // 指定属性的 DISP ID
    string RemoteAddresses
    {
        [DispId(5)]  // 指定 get 方法的 DISP ID
        [MethodImpl(MethodImplOptions.InternalCall)]  // 指示方法实现由 COM 提供
        [return: MarshalAs(UnmanagedType.BStr)]  // 指定返回值类型为 BStr
        get;  // 属性的 getter 方法
        [DispId(5)]  // 指定 set 方法的 DISP ID
        [MethodImpl(MethodImplOptions.InternalCall)]  // 指示方法实现由 COM 提供
        [param: MarshalAs(UnmanagedType.BStr)]  // 指定参数类型为 BStr
        set;  // 属性的 setter 方法
    }

    // 定义 Enabled 属性，具有 get 和 set 方法
    [DispId(6)]  // 指定属性的 DISP ID
    bool Enabled
    {
        [DispId(6)]  // 指定 get 方法的 DISP ID
        [MethodImpl(MethodImplOptions.InternalCall)]  // 指示方法实现由 COM 提供
        get;  // 属性的 getter 方法
        [DispId(6)]  // 指定 set 方法的 DISP ID
        [MethodImpl(MethodImplOptions.InternalCall)]  // 指示方法实现由 COM 提供
        set;  // 属性的 setter 方法
    }
}
```