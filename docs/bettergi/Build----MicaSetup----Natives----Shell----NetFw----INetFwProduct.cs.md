# `.\better-genshin-impact\Build\MicaSetup\Natives\Shell\NetFw\INetFwProduct.cs`

```cs
# 引入系统基础类库
using System;
# 引入运行时编译器服务库
using System.Runtime.CompilerServices;
# 引入运行时互操作服务库
using System.Runtime.InteropServices;

# 定义命名空间 MicaSetup.Shell.NetFw
namespace MicaSetup.Shell.NetFw;

# 为接口 INetFwProduct 指定唯一的 GUID 和 COM 类型库类型
[Guid("71881699-18F4-458B-B892-3FFCE5E07F75"), TypeLibType(4160)]
# 标记此接口为 COM 组件
[ComImport]
public interface INetFwProduct
{
    # 定义一个属性 RuleCategories，DispId 为 1
    [DispId(1)]
    object RuleCategories
    {
        # 获取 RuleCategories 属性
        [DispId(1)]
        [MethodImpl(MethodImplOptions.InternalCall)]
        [return: MarshalAs(UnmanagedType.Struct)]
        get;
        # 设置 RuleCategories 属性
        [DispId(1)]
        [MethodImpl(MethodImplOptions.InternalCall)]
        [param: MarshalAs(UnmanagedType.Struct)]
        set;
    }

    # 定义一个属性 DisplayName，DispId 为 2
    [DispId(2)]
    string DisplayName
    {
        # 获取 DisplayName 属性
        [DispId(2)]
        [MethodImpl(MethodImplOptions.InternalCall)]
        [return: MarshalAs(UnmanagedType.BStr)]
        get;
        # 设置 DisplayName 属性
        [DispId(2)]
        [MethodImpl(MethodImplOptions.InternalCall)]
        [param: MarshalAs(UnmanagedType.BStr)]
        set;
    }

    # 定义一个属性 PathToSignedProductExe，DispId 为 3
    [DispId(3)]
    string PathToSignedProductExe
    {
        # 获取 PathToSignedProductExe 属性
        [DispId(3)]
        [MethodImpl(MethodImplOptions.InternalCall)]
        [return: MarshalAs(UnmanagedType.BStr)]
        get;
    }
}
```