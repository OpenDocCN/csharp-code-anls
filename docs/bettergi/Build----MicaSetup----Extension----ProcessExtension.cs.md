# `.\better-genshin-impact\Build\MicaSetup\Extension\ProcessExtension.cs`

```cs
# 引用必需的命名空间
﻿using System.Collections.Generic;
using System.Diagnostics;
using System.Diagnostics.CodeAnalysis;
using System.IO;
using System.Text;

namespace MicaSetup.Helper;

# 定义一个静态类用于扩展 FluentProcess 类
public static class ProcessExtension
{
    # 设置要执行的文件名并返回 FluentProcess 对象
    public static FluentProcess FileName(this FluentProcess self, string filename)
    {
        self.StartInfo.FileName = filename; # 将文件名赋给 StartInfo 的 FileName 属性
        return self; # 返回 FluentProcess 对象
    }

    # 设置命令行参数并返回 FluentProcess 对象
    public static FluentProcess Arguments(this FluentProcess self, string args)
    {
        self.StartInfo.Arguments = args; # 将参数赋给 StartInfo 的 Arguments 属性
        return self; # 返回 FluentProcess 对象
    }

    # 设置是否在不显示窗口的情况下启动进程并返回 FluentProcess 对象
    public static FluentProcess CreateNoWindow(this FluentProcess self, bool enabled = true)
    {
        self.StartInfo.CreateNoWindow = enabled; # 设置 StartInfo 的 CreateNoWindow 属性
        return self; # 返回 FluentProcess 对象
    }

    # 添加环境变量并返回 FluentProcess 对象
    public static FluentProcess Environment(this FluentProcess self, params KeyValuePair<string, string>[] vars)
    {
        foreach (var v in vars)
        {
            self.StartInfo.Environment.Add(v!); # 向 StartInfo 的 Environment 添加环境变量
        }
        return self; # 返回 FluentProcess 对象
    }

    # 添加环境变量并返回 Process 对象
    public static Process EnvironmentVariables(this FluentProcess self, params KeyValuePair<string, string>[] vars)
    {
        foreach (var v in vars)
        {
            self.StartInfo.EnvironmentVariables.Add(v.Key, v.Value); # 向 StartInfo 的 EnvironmentVariables 添加环境变量
        }
        return self; # 返回 Process 对象
    }

    # 设置是否重定向标准错误流并返回 FluentProcess 对象
    public static FluentProcess RedirectStandardError(this FluentProcess self, bool enabled = true)
    {
        self.StartInfo.RedirectStandardError = enabled; # 设置 StartInfo 的 RedirectStandardError 属性
        return self; # 返回 FluentProcess 对象
    }

    # 设置是否重定向标准输入流并返回 FluentProcess 对象
    public static FluentProcess RedirectStandardInput(this FluentProcess self, bool enabled = true)
    {
        self.StartInfo.RedirectStandardInput = enabled; # 设置 StartInfo 的 RedirectStandardInput 属性
        return self; # 返回 FluentProcess 对象
    }

    # 设置是否重定向标准输出流并返回 FluentProcess 对象
    public static FluentProcess RedirectStandardOutput(this FluentProcess self, bool enabled = true)
    {
        self.StartInfo.RedirectStandardOutput = enabled; # 设置 StartInfo 的 RedirectStandardOutput 属性
        return self; # 返回 FluentProcess 对象
    }

    # 设置标准错误流的编码并返回 FluentProcess 对象
    public static FluentProcess StandardErrorEncoding(this FluentProcess self, Encoding encoding = null!)
    {
        self.StartInfo.StandardErrorEncoding = encoding ?? Encoding.Default; # 设置 StartInfo 的 StandardErrorEncoding 属性
        return self; # 返回 FluentProcess 对象
    }

    # 设置标准输出流的编码并返回 FluentProcess 对象
    public static FluentProcess StandardOutputEncoding(this FluentProcess self, Encoding encoding = null!)
    {
        self.StartInfo.StandardOutputEncoding = encoding ?? Encoding.Default; # 设置 StartInfo 的 StandardOutputEncoding 属性
        return self; # 返回 FluentProcess 对象
    }

    # 开始读取标准输出流并将其复制到指定的流中
    public static FluentProcess BeginOutputRead(this FluentProcess self, Stream stream)
    {
        long pos = stream.Position; # 保存当前流的位置
        self.StandardOutput.BaseStream.CopyTo(stream); # 将标准输出流的数据复制到指定流
        stream.Position = pos; # 恢复流的位置
        return self; # 返回 FluentProcess 对象
    }

    # 开始读取标准错误流并将其复制到指定的流中
    public static FluentProcess BeginErrorRead(this FluentProcess self, Stream stream)
    {
        long pos = stream.Position; # 保存当前流的位置
        self.StandardOutput.BaseStream.CopyTo(stream); # 将标准错误流的数据复制到指定流
        stream.Position = pos; # 恢复流的位置
        return self; # 返回 FluentProcess 对象
    }

    # 设置是否使用操作系统外壳程序来启动进程并返回 FluentProcess 对象
    public static FluentProcess UseShellExecute(this FluentProcess self, bool enabled = true)
    {
        self.StartInfo.UseShellExecute = enabled; # 设置 StartInfo 的 UseShellExecute 属性
        return self; # 返回 FluentProcess 对象
    }

    # 设置进程的动词（如 "open" 或 "edit"）并返回 FluentProcess 对象
    public static FluentProcess Verb(this FluentProcess self, string verb)
    # 设置启动信息的操作类型（例如 "runas"）
    self.StartInfo.Verb = verb;
    # 返回当前 FluentProcess 对象，以便链式调用
    return self;

    # 设置 FluentProcess 对象的工作目录
    public static FluentProcess WorkingDirectory(this FluentProcess self, string directory)
    {
        # 将工作目录设置为指定目录
        self.StartInfo.WorkingDirectory = directory;
        # 返回当前 FluentProcess 对象，以便链式调用
        return self;
    }

    # 设置在接收到输出数据时调用的事件处理程序
    public static FluentProcess SetOutputDataReceived(this FluentProcess self, DataReceivedEventHandler handler)
    {
        # 将事件处理程序附加到 OutputDataReceived 事件
        self.OutputDataReceived += handler;
        # 返回当前 FluentProcess 对象，以便链式调用
        return self;
    }

    # 设置在接收到错误数据时调用的事件处理程序
    public static FluentProcess SetErrorDataReceived(this FluentProcess self, DataReceivedEventHandler handler)
    {
        # 将事件处理程序附加到 ErrorDataReceived 事件
        self.ErrorDataReceived += handler;
        # 返回当前 FluentProcess 对象，以便链式调用
        return self;
    }

    # 用于忽略警告的空方法
    [SuppressMessage("Style", "IDE0060:")]
    public static void Forget(this FluentProcess self)
    {
        # 方法体为空
    }
# 结束大括号，标识类定义的结束
}

# 定义 FluentProcess 类，继承自 Process 类
public class FluentProcess : Process
{
    # 静态方法，创建 FluentProcess 的新实例
    public static FluentProcess Create()
    {
        return new FluentProcess();
    }

    # 新的静态方法，设置文件名并启动进程
    public new static FluentProcess Start(string fileName)
    {
        return Create()
            .FileName(fileName)  # 设置进程的文件名
            .Start();  # 启动进程
    }

    # 新的静态方法，设置文件名和参数并启动进程
    public new static FluentProcess Start(string fileName, string args)
    {
        return new FluentProcess()
            .FileName(fileName)  # 设置进程的文件名
            .Arguments(args)  # 设置进程的启动参数
            .Start();  # 启动进程
    }

    # 新的方法，开始异步读取错误输出
    public new FluentProcess BeginErrorReadLine()
    {
        base.BeginErrorReadLine();  # 调用基类的 BeginErrorReadLine 方法
        return this;  # 返回当前实例
    }

    # 新的方法，开始异步读取标准输出
    public new FluentProcess BeginOutputReadLine()
    {
        base.BeginOutputReadLine();  # 调用基类的 BeginOutputReadLine 方法
        return this;  # 返回当前实例
    }

    # 新的方法，等待进程退出
    public new FluentProcess WaitForExit()
    {
        base.WaitForExit();  # 调用基类的 WaitForExit 方法
        return this;  # 返回当前实例
    }

    # 新的方法，等待进程的输入闲置
    public new FluentProcess WaitForInputIdle()
    {
        base.WaitForInputIdle();  # 调用基类的 WaitForInputIdle 方法
        return this;  # 返回当前实例
    }

    # 新的方法，启动进程并记录信息
    public new FluentProcess Start()
    {
        Logger.Info(
        $"""
            "{StartInfo.FileName}" {StartInfo.Arguments}
        """);  # 记录进程启动的信息
        base.Start();  # 调用基类的 Start 方法
        return this;  # 返回当前实例
    }
}
```