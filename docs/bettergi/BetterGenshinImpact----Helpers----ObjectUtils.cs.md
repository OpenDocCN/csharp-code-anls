# `.\better-genshin-impact\BetterGenshinImpact\Helpers\ObjectUtils.cs`

```cs
# 引入所需的命名空间
﻿using System;
using System.IO;
using System.Runtime.Serialization.Formatters.Binary;

namespace BetterGenshinImpact.Helpers;

public class ObjectUtils
{
    # 标记该方法为过时的
    [Obsolete("Obsolete")]
    public static byte[] Serialize(object obj)
    {
        # 创建一个内存流对象
        var ms = new MemoryStream();
        # 创建一个二进制格式化程序
        var formatter = new BinaryFormatter();
#pragma warning disable SYSLIB0011
        # 使用二进制格式化程序将对象序列化到内存流中
        formatter.Serialize(ms, obj);
#pragma warning restore SYSLIB0011
        # 获取内存流中的字节数组
        byte[] bytes = ms.GetBuffer();
        # 返回字节数组
        return bytes;
    }

    # 标记该方法为过时的
    [Obsolete("Obsolete")]
    public static object Deserialize(byte[] bytes)
    {
        # 利用传来的字节数组创建一个内存流对象，并设置流的位置为0
        var ms = new MemoryStream(bytes)
        {
            Position = 0
        };
        # 创建一个二进制格式化程序
        var formatter = new BinaryFormatter();
#pragma warning disable SYSLIB0011
        # 使用二进制格式化程序将内存流反序列化为对象
        var obj = formatter.Deserialize(ms);
#pragma warning restore SYSLIB0011
        # 关闭内存流对象
        ms.Close();
        # 返回反序列化后的对象
        return obj;
    }
}
```