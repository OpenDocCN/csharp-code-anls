# `.\better-genshin-impact\Build\MicaSetup\Natives\Shell\Dialogs\Common\ShellException.cs`

```cs
# 引入系统命名空间
﻿using System;
# 引入运行时互操作性服务的命名空间
using System.Runtime.InteropServices;

# 声明命名空间
namespace MicaSetup.Shell.Dialogs;

# 指定该类可以被序列化
[Serializable]
# 定义一个继承自 ExternalException 的异常类
public class ShellException : ExternalException
{
    # 默认构造函数
    public ShellException()
    {
    }

    # 带有自定义错误消息的构造函数
    public ShellException(string message) : base(message)
    {
    }

    # 带有自定义错误消息和内部异常的构造函数
    public ShellException(string message, Exception innerException)
        : base(message, innerException)
    {
    }

    # 带有自定义错误消息和错误代码的构造函数
    public ShellException(string message, int errorCode) : base(message, errorCode)
    {
    }

    # 带有错误代码的构造函数
    public ShellException(int errorCode)
        : base(LocalizedMessages.ShellExceptionDefaultText, errorCode)
    {
    }

    # 使用 HResult 构造函数
    internal ShellException(HResult result) : this((int)result)
    {
    }

    # 使用自定义错误消息和 HResult 构造函数
    internal ShellException(string message, HResult errorCode) : this(message, (int)errorCode)
    {
    }

    # 受保护的构造函数用于序列化
    protected ShellException(
        System.Runtime.Serialization.SerializationInfo info,
        System.Runtime.Serialization.StreamingContext context)
        : base(info, context)
    {
    }
}
```