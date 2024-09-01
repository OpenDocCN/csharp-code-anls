# `.\better-genshin-impact\Build\MicaSetup\Design\Hosts\Hosting.cs`

```cs
# 定义命名空间 MicaSetup.Design.Controls
﻿namespace MicaSetup.Design.Controls;

# 定义一个静态类 Hosting
public static class Hosting
{
    # 定义一个静态方法 CreateBuilder，返回类型为 IHostBuilder
    public static IHostBuilder CreateBuilder()
    {
        # 创建一个 HostBuilder 实例
        HostBuilder builder = new();
        # 返回创建的 HostBuilder 实例
        return builder;
    }
}
```