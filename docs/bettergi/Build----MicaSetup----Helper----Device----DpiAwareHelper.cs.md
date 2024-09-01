# `.\better-genshin-impact\Build\MicaSetup\Helper\Device\DpiAwareHelper.cs`

```cs
# 引用 MicaSetup.Helper 命名空间中的帮助程序
﻿using MicaSetup.Helper;

# 声明一个名为 MicaSetup.Natives 的命名空间
namespace MicaSetup.Natives;

# 声明一个公共静态类 DpiAwareHelper
public static class DpiAwareHelper
{
    # 声明一个公共静态方法 SetProcessDpiAwareness，返回布尔值
    public static bool SetProcessDpiAwareness()
    {
        # 如果操作系统版本是 Windows 8.1 或更高版本
        if (OsVersionHelper.IsWindows81_OrGreater)
        {
            # 尝试设置进程 DPI 感知为每个显示器 DPI 感知
            if (SHCore.SetProcessDpiAwareness(PROCESS_DPI_AWARENESS.PROCESS_PER_MONITOR_DPI_AWARE) == 0)
            {
                # 如果设置成功，返回 true
                return true;
            }
        }
        # 如果操作系统版本较低或设置失败，返回 false
        return false;
    }
}
```