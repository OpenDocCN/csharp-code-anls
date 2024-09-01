# `.\better-genshin-impact\Build\MicaSetup\Natives\Shell\Dialogs\PropertySystem\PropertySystemException.cs`

```cs
# 引入系统库和 COM 相关功能的命名空间
using System;
using System.Runtime.InteropServices;

# 定义命名空间 MicaSetup.Shell.Dialogs
namespace MicaSetup.Shell.Dialogs;

# 标记此类可以被序列化
[Serializable]
# 定义一个名为 PropertySystemException 的类，继承自 ExternalException
public class PropertySystemException : ExternalException
{
    # 无参数的构造函数
    public PropertySystemException()
    {
    }

    # 带有错误消息的构造函数
    public PropertySystemException(string message) : base(message)
    {
    }

    # 带有错误消息和内部异常的构造函数
    public PropertySystemException(string message, Exception innerException)
        : base(message, innerException)
    {
    }

    # 带有错误消息和错误代码的构造函数
    public PropertySystemException(string message, int errorCode) : base(message, errorCode)
    {
    }

    # 受保护的构造函数，用于序列化时恢复状态
    protected PropertySystemException(
        System.Runtime.Serialization.SerializationInfo info,
        System.Runtime.Serialization.StreamingContext context)
        : base(info, context)
    {
    }
}
```