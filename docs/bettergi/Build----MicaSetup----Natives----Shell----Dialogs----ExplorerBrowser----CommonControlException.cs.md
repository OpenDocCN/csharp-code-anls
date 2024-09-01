# `.\better-genshin-impact\Build\MicaSetup\Natives\Shell\Dialogs\ExplorerBrowser\CommonControlException.cs`

```cs
# 使用 C# 的 System 命名空间和 System.Runtime.InteropServices 命名空间
﻿using System;
using System.Runtime.InteropServices;

# 定义一个命名空间 MicaSetup.Shell.Dialogs
namespace MicaSetup.Shell.Dialogs;

# 使 CommonControlException 类可序列化
[Serializable]
# 定义一个 CommonControlException 类，继承自 COMException
public class CommonControlException : COMException
{
    # 默认构造函数
    public CommonControlException()
    {
    }

    # 带有消息的构造函数
    public CommonControlException(string message) : base(message)
    {
    }

    # 带有消息和内部异常的构造函数
    public CommonControlException(string message, Exception innerException)
        : base(message, innerException)
    {
    }

    # 带有消息和错误代码的构造函数
    public CommonControlException(string message, int errorCode) : base(message, errorCode)
    {
    }

    # 内部构造函数，接受消息和 HResult 错误代码，将 HResult 转换为 int 类型
    internal CommonControlException(string message, HResult errorCode) : this(message, (int)errorCode)
    {
    }

    # 保护构造函数，用于序列化
    protected CommonControlException(
        System.Runtime.Serialization.SerializationInfo info,
        System.Runtime.Serialization.StreamingContext context)
        : base(info, context)
    {
    }
}
```