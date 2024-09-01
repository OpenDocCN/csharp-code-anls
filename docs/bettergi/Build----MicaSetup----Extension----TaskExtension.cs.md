# `.\better-genshin-impact\Build\MicaSetup\Extension\TaskExtension.cs`

```cs
# 引入用于代码分析和编译器的功能
﻿using System.Diagnostics.CodeAnalysis;
using System.Runtime.CompilerServices;
using System.Threading.Tasks;

# 定义一个静态类用于扩展任务功能
namespace MicaSetup.Helper;

public static class TaskExtension
{
    # 忽略未使用参数的警告
    [SuppressMessage("Style", "IDE0060:")]
    # 使用内联编译方法提高性能
    [MethodImpl(MethodImplOptions.AggressiveInlining)]
    # 扩展 Task 类，提供一个无操作的“忘记”方法
    public static void Forget(this Task self)
    {
    }

    # 忽略未使用参数的警告
    [SuppressMessage("Style", "IDE0060:")]
    # 使用内联编译方法提高性能
    [MethodImpl(MethodImplOptions.AggressiveInlining)]
    # 扩展 ConfiguredTaskAwaitable 类，提供一个无操作的“忘记”方法
    public static void Forget(this ConfiguredTaskAwaitable self)
    {
    }
}
```