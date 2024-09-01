# `.\better-genshin-impact\Build\MicaSetup\Natives\NTdll.cs`

```cs
# 导入系统运行时库用于互操作性
﻿using System.Runtime.InteropServices;
# 导入系统安全相关功能
using System.Security;

# 定义命名空间
namespace MicaSetup.Natives;

# 定义一个静态类 NTdll
public static class NTdll
{
    # 指定该方法为安全关键，调用时需要注意安全性
    [SecurityCritical]
    # 声明外部方法 RtlGetVersion，指定 DLL 名称、设置错误标志和字符集
    [DllImport(Lib.NTdll, SetLastError = true, CharSet = CharSet.Unicode)]
    # 指定 DLL 导入搜索路径为系统32目录
    [DefaultDllImportSearchPaths(DllImportSearchPath.System32)]
    # 声明外部方法 RtlGetVersion，返回值为整数，输出参数为 OSVERSIONINFOEX 类型
    public static extern int RtlGetVersion(out OSVERSIONINFOEX versionInfo);
}
```