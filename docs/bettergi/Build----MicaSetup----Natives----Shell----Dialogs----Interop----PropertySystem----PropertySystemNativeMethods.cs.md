# `.\better-genshin-impact\Build\MicaSetup\Natives\Shell\Dialogs\Interop\PropertySystem\PropertySystemNativeMethods.cs`

```cs
﻿using MicaSetup.Natives;  // 引入 MicaSetup.Natives 命名空间
using System;  // 引入 System 命名空间
using System.Runtime.InteropServices;  // 引入用于平台调用（P/Invoke）的命名空间

namespace MicaSetup.Shell.Dialogs;  // 定义一个命名空间 MicaSetup.Shell.Dialogs

public static class PropertySystemNativeMethods  // 定义一个静态类 PropertySystemNativeMethods
{
    // 枚举，定义了各种相对描述类型
    public enum RelativeDescriptionType
    {
        General,  // 一般描述
        Date,  // 日期描述
        Size,  // 大小描述
        Count,  // 数量描述
        Revision,  // 修订描述
        Length,  // 长度描述
        Duration,  // 持续时间描述
        Speed,  // 速度描述
        Rate,  // 比率描述
        Rating,  // 评级描述
        Priority  // 优先级描述
    }

    // 定义一个 P/Invoke 方法，用于从属性键获取属性的规范名称
    [DllImport(Lib.PropSys, CharSet = CharSet.Unicode, SetLastError = true)]
    public static extern int PSGetNameFromPropertyKey(
        ref PropertyKey propkey,  // 输入参数，属性键的引用
        [Out, MarshalAs(UnmanagedType.LPWStr)] out string ppszCanonicalName  // 输出参数，属性的规范名称
    );

    // 定义一个 P/Invoke 方法，用于从属性键获取属性描述
    [DllImport(Lib.PropSys, CharSet = CharSet.Unicode, SetLastError = true)]
    public static extern HResult PSGetPropertyDescription(
        ref PropertyKey propkey,  // 输入参数，属性键的引用
        ref Guid riid,  // 输入参数，接口标识符（GUID）的引用
        [Out, MarshalAs(UnmanagedType.Interface)] out IPropertyDescription ppv  // 输出参数，属性描述接口
    );

    // 定义一个 P/Invoke 方法，用于从字符串获取属性描述列表
    [DllImport(Lib.PropSys, CharSet = CharSet.Unicode, SetLastError = true)]
    public static extern int PSGetPropertyDescriptionListFromString(
        [In, MarshalAs(UnmanagedType.LPWStr)] string pszPropList,  // 输入参数，属性描述列表的字符串
        [In] ref Guid riid,  // 输入参数，接口标识符（GUID）的引用
        out IPropertyDescriptionList ppv  // 输出参数，属性描述列表接口
    );

    // 定义一个 P/Invoke 方法，用于从属性规范名称获取属性键
    [DllImport(Lib.PropSys, CharSet = CharSet.Unicode, SetLastError = true)]
    public static extern int PSGetPropertyKeyFromName(
        [In, MarshalAs(UnmanagedType.LPWStr)] string pszCanonicalName,  // 输入参数，属性的规范名称
        out PropertyKey propkey  // 输出参数，属性键
    );
}
```