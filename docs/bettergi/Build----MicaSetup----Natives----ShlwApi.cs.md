# `.\better-genshin-impact\Build\MicaSetup\Natives\ShlwApi.cs`

```cs
# 引用 System.Runtime.InteropServices 命名空间以使用 DllImport 和相关的特性
﻿using System.Runtime.InteropServices;

# 定义命名空间 MicaSetup.Natives
namespace MicaSetup.Natives;

# 定义一个静态类 ShlwApi
public static class ShlwApi
{
    # 使用 DllImport 特性来声明外部方法 PathParseIconLocation，指定库名和字符集
    [DllImport(Lib.ShlwApi, CharSet = CharSet.Unicode, SetLastError = true)]
    # 声明 PathParseIconLocation 方法为外部方法，接受一个引用类型的字符串参数，返回一个整数
    public static extern int PathParseIconLocation([MarshalAs(UnmanagedType.LPWStr)] ref string pszIconFile);
}
```