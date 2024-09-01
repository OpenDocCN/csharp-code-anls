# `.\better-genshin-impact\Build\MicaSetup\Natives\Shell\NetFw\INetFwPolicy.cs`

```cs
# 引入必要的命名空间
using System;
using System.Runtime.CompilerServices;
using System.Runtime.InteropServices;

namespace MicaSetup.Shell.NetFw;

# 定义 COM 接口 INetFwPolicy，GUID 和 TypeLibType 属性用于标识该接口
[Guid("D46D2478-9AC9-4008-9DC7-5563CE5536CC"), TypeLibType(4160)]
[ComImport]
public interface INetFwPolicy
{
    # 定义属性 CurrentProfile，表示当前的防火墙配置文件
    [DispId(1)]
    INetFwProfile CurrentProfile
    {
        # 属性访问器，用于获取当前配置文件
        [DispId(1)]
        [MethodImpl(MethodImplOptions.InternalCall)]
        [return: MarshalAs(UnmanagedType.Interface)]
        get;
    }

    # 定义方法 GetProfileByType，按类型获取特定的防火墙配置文件
    [DispId(2)]
    [MethodImpl(MethodImplOptions.InternalCall)]
    [return: MarshalAs(UnmanagedType.Interface)]
    INetFwProfile GetProfileByType([In] NET_FW_PROFILE_TYPE profileType);
}
```