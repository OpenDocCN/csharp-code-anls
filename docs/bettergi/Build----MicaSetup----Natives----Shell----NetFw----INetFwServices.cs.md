# `.\better-genshin-impact\Build\MicaSetup\Natives\Shell\NetFw\INetFwServices.cs`

```cs
# 使用 System 命名空间中的类和接口
using System;
using System.Collections;
using System.Runtime.CompilerServices;
using System.Runtime.InteropServices;

# 定义一个名为 MicaSetup.Shell.NetFw 的命名空间
namespace MicaSetup.Shell.NetFw;

# 禁用特定的编译器警告
#pragma warning disable CS0108

# 定义一个 COM 接口 INetFwServices，并指定 GUID 和类型库类型
[Guid("79649BB4-903E-421B-94C9-79848E79F6EE"), TypeLibType(4160)]
[ComImport]
public interface INetFwServices : IEnumerable
{
    # 定义一个 Count 属性，用于获取服务的数量
    [DispId(1)]
    int Count
    {
        [DispId(1)]
        [MethodImpl(MethodImplOptions.InternalCall)]
        get;
    }

    # 定义一个 Item 方法，用于根据服务类型获取服务实例
    [DispId(2)]
    [MethodImpl(MethodImplOptions.InternalCall)]
    [return: MarshalAs(UnmanagedType.Interface)]
    INetFwService Item([In] NET_FW_SERVICE_TYPE svcType);

    # 定义一个 GetEnumerator 方法，用于获取服务的枚举器
    [DispId(-4), TypeLibFunc(1)]
    [MethodImpl(MethodImplOptions.InternalCall)]
    [return: MarshalAs(UnmanagedType.CustomMarshaler, MarshalType = "System.Runtime.InteropServices.CustomMarshalers.EnumeratorToEnumVariantMarshaler")]
    IEnumerator GetEnumerator();
}
```