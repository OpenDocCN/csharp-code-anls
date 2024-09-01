# `.\better-genshin-impact\Build\MicaSetup\Natives\Shell\NetFw\INetFwRules.cs`

```cs
# 引入所需的命名空间
using System;
using System.Collections;
using System.Runtime.CompilerServices;
using System.Runtime.InteropServices;

namespace MicaSetup.Shell.NetFw;

# 禁用指定的编译器警告
#pragma warning disable CS0108

# 定义 COM 接口 INetFwRules，继承 IEnumerable 接口
[Guid("9C4C6277-5027-441E-AFAE-CA1F542DA009"), TypeLibType(4160)]
[ComImport]
public interface INetFwRules : IEnumerable
{
    # 属性，获取规则的数量
    [DispId(1)]
    int Count
    {
        [DispId(1)]
        [MethodImpl(MethodImplOptions.InternalCall)]
        get;
    }

    # 方法，添加一个防火墙规则
    [DispId(2)]
    [MethodImpl(MethodImplOptions.InternalCall)]
    void Add([MarshalAs(UnmanagedType.Interface)][In] INetFwRule rule);

    # 方法，移除指定名称的防火墙规则
    [DispId(3)]
    [MethodImpl(MethodImplOptions.InternalCall)]
    void Remove([MarshalAs(UnmanagedType.BStr)][In] string Name);

    # 方法，获取指定名称的防火墙规则
    [DispId(4)]
    [MethodImpl(MethodImplOptions.InternalCall)]
    [return: MarshalAs(UnmanagedType.Interface)]
    INetFwRule Item([MarshalAs(UnmanagedType.BStr)][In] string Name);

    # 方法，获取规则的枚举器
    [DispId(-4), TypeLibFunc(1)]
    [MethodImpl(MethodImplOptions.InternalCall)]
    [return: MarshalAs(UnmanagedType.CustomMarshaler, MarshalType = "System.Runtime.InteropServices.CustomMarshalers.EnumeratorToEnumVariantMarshaler")]
    IEnumerator GetEnumerator();
}
```