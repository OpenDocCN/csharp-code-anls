# `.\better-genshin-impact\Build\MicaSetup\Natives\Gdi32.cs`

```cs
# 引入系统运行时导入库，用于调用非托管代码
﻿using System.Runtime.InteropServices;

# 命名空间定义
namespace MicaSetup.Natives;

# 定义静态类 Gdi32，用于包装 GDI32 库的函数
public static class Gdi32
{
    # 从 GDI32 库中导入 GetDeviceCaps 函数，设置错误标志为 false，精确匹配为 true
    [DllImport(Lib.Gdi32, SetLastError = false, ExactSpelling = true)]
    # 定义 GetDeviceCaps 函数的外部声明，返回类型为 int
    public static extern int GetDeviceCaps(nint hdc, DeviceCap nIndex);

    # 从 GDI32 库中导入 CreateDC 函数，设置错误标志为 false，字符集为自动
    [DllImport(Lib.Gdi32, SetLastError = false, CharSet = CharSet.Auto)]
    # 定义 CreateDC 函数的外部声明，返回类型为 nint
    public static extern nint CreateDC([Optional] string pwszDriver, [Optional] string pwszDevice, [Optional] string pszPort, object pdm);

    # 从 GDI32 库中导入 DeleteObject 函数
    [DllImport(Lib.Gdi32)]
    # 定义 DeleteObject 函数的外部声明，返回值为 bool 类型
    [return: MarshalAs(UnmanagedType.Bool)]
    public static extern bool DeleteObject(nint hObject);
}
```