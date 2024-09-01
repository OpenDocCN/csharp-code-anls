# `.\better-genshin-impact\Build\MicaSetup\Natives\SHCore.cs`

```cs
# 导入用于访问系统级别功能的 COM 相关程序集
﻿using System.Runtime.InteropServices;

# 定义一个命名空间，用于包含与 MicaSetup 相关的本地方法
namespace MicaSetup.Natives;

# 定义一个静态类 SHCore，包含与 SHCore 库相关的外部方法
public static class SHCore
{
    # 导入 SHCore 库中的 SetProcessDpiAwareness 方法，用于设置进程的 DPI 感知级别
    [DllImport(Lib.SHCore)]
    public static extern uint SetProcessDpiAwareness(PROCESS_DPI_AWARENESS awareness);

    # 导入 SHCore 库中的 GetDpiForMonitor 方法，用于获取指定监视器的 DPI 值
    [DllImport(Lib.SHCore)]
    public static extern int GetDpiForMonitor(nint hmonitor, MONITOR_DPI_TYPE dpiType, out uint dpiX, out uint dpiY);
}
```