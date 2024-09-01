# `.\better-genshin-impact\Build\MicaSetup\Helper\Device\ConnectivityHelper.cs`

```cs
# 引入网络接口相关的命名空间
﻿using System.Net.NetworkInformation;

# 定义一个静态类用于网络连接帮助函数
namespace MicaSetup.Helper;

public static class ConnectivityHelper
{
    # 静态属性，检查网络是否可用
    public static bool IsNetworkAvailable => NetworkInterface.GetIsNetworkAvailable();

    # 静态方法，尝试对指定的主机名或地址进行 ping 操作
    public static bool Ping(string hostNameOrAddress = null!)
    {
        try
        {
            # 创建一个新的 Ping 实例
            using Ping ping = new();
            # 发送 ping 请求到指定的主机名或地址，如果没有提供则默认为 "www.microsoft.com"
            PingReply reply = ping.Send(hostNameOrAddress ?? "www.microsoft.com");
            # 如果 ping 操作成功，返回 true，否则返回 false
            return reply?.Status == IPStatus.Success;
        }
        catch
        {
            # 捕获异常但不做处理，默认返回 false
        }
        # 如果发生异常或 ping 失败，返回 false
        return false;
    }
}
```