# `.\better-genshin-impact\Build\MicaSetup\Core\Logger.cs`

```cs
# 引入所需的命名空间和类
﻿using MicaSetup.Helper;
using System;
using System.Diagnostics;
using System.Diagnostics.CodeAnalysis;
using System.Globalization;
using System.IO;
using System.Reflection;
using System.Text;
using DebugOut = System.Diagnostics.Debug;

namespace MicaSetup.Core;

# 定义静态日志记录类
public static class Logger
{
    # 定义静态只读字段，表示应用程序日志文件夹路径
    private static readonly string ApplicationLogPath = SpecialPathHelper.GetFolder();
    # 定义静态只读字段，表示跟踪监听器
    private static readonly TextWriterTraceListener TraceListener = null!;

    # 静态构造函数，初始化日志记录器
    static Logger()
    {
        # 如果日志文件夹不存在，则创建它
        if (!Directory.Exists(ApplicationLogPath))
        {
            _ = Directory.CreateDirectory(ApplicationLogPath);
        }

        # 生成日志文件路径，并创建新的跟踪监听器
        string logFilePath = Path.Combine(ApplicationLogPath, DateTime.Now.ToString(@"yyyyMMdd", CultureInfo.InvariantCulture) + ".log");
        TraceListener = new TextWriterTraceListener(logFilePath);
#if LEGACY
        # 如果处于遗留模式，启用自动刷新并设置跟踪监听器
        Trace.AutoFlush = true;
        Trace.Listeners.Clear();
        Trace.Listeners.Add(TraceListener);
#endif
    }

    # 忽略方法，不进行任何操作
    [SuppressMessage("Style", "IDE0060:")]
    public static void Ignore(params object[] values)
    {
    }

    # 条件编译调试方法，只有在DEBUG模式下才会编译
    [Conditional("DEBUG")]
    public static void Debug(params object[] values)
    {
        # 调用日志方法，记录DEBUG级别的消息
        Log("DEBUG", string.Join(" ", values));
    }

    # 记录INFO级别的日志
    public static void Info(params object[] values)
    {
        Log("INFO", string.Join(" ", values));
    }

    # 记录WARN级别的日志
    public static void Warn(params object[] values)
    {
        Log("ERROR", string.Join(" ", values));
    }

    # 记录ERROR级别的日志
    public static void Error(params object[] values)
    {
        Log("ERROR", string.Join(" ", values));
    }

    # 记录FATAL级别的日志
    public static void Fatal(params object[] values)
    {
        Log("FATAL", string.Join(" ", values));
    }

    # 记录异常信息及其堆栈跟踪
    public static void Exception(Exception e, string message = null!)
    {
        Log(
            (message ?? string.Empty) + Environment.NewLine +
            e?.Message + Environment.NewLine +
            "Inner exception: " + Environment.NewLine +
            e?.InnerException?.Message + Environment.NewLine +
            "Stack trace: " + Environment.NewLine +
            e?.StackTrace,
            "ERROR");
#if DEBUG
        # 在DEBUG模式下，触发调试器中断
        Debugger.Break();
#endif
    }

    # 记录日志的核心方法
    private static void Log(string type, string message)
    {
        # 创建一个StringBuilder实例，构造日志消息
        StringBuilder sb = new();

        sb.Append(type + "|" + DateTime.Now.ToString(@"yyyy-MM-dd|HH:mm:ss.fff", CultureInfo.InvariantCulture))
          .Append("|" + GetCallerInfo())
          .Append("|" + message);

        # 输出日志到调试输出
        DebugOut.WriteLine(sb.ToString());
        # 如果当前选项允许日志记录，将日志写入文件并刷新
        if (Option.Current.Logging)
        {
            TraceListener.WriteLine(sb.ToString());
            TraceListener.Flush();
        }
    }

    # 获取调用日志记录方法的类名和方法名
    private static string GetCallerInfo()
    {
        StackTrace stackTrace = new();

        MethodBase methodName = stackTrace.GetFrame(3)?.GetMethod()!;
        string? className = methodName?.DeclaringType?.Name;
        return className + "|" + methodName?.Name;
    }
}
```