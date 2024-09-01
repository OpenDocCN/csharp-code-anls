# `.\better-genshin-impact\Build\MicaSetup\Natives\Shell32.cs`

```cs
# 引入系统运行时库，提供与 Windows API 交互的功能
﻿using System.Runtime.InteropServices;
# 引入安全库，以确保对不受信任代码的安全处理
using System.Security;

# 定义命名空间，组织相关的类
namespace MicaSetup.Natives;

# 定义静态类 Shell32，用于封装与 Shell32.dll 的交互
public static class Shell32
{
    # 导入 Shell32.dll 中的 SHChangeNotify 方法，用于通知系统文件系统的更改
    [DllImport(Lib.Shell32, CharSet = CharSet.Auto, SetLastError = true)]
    public static extern void SHChangeNotify(SHCNE wEventId, SHCNF uFlags, nint dwItem1, nint dwItem2);

    # 导入 Shell32.dll 中的 SHAddToRecentDocs 方法，用于将文档添加到最近使用的文件列表
    [DllImport(Lib.Shell32, ExactSpelling = true, SetLastError = true)]
    [SecurityCritical, SuppressUnmanagedCodeSecurity]
    public static extern void SHAddToRecentDocs(SHARD uFlags, [MarshalAs(UnmanagedType.LPWStr)] string pv);
}
```