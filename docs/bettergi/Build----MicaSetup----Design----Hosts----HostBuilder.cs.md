# `.\better-genshin-impact\Build\MicaSetup\Design\Hosts\HostBuilder.cs`

```cs
# 引入 Microsoft.Extensions.DependencyInjection 命名空间
﻿using Microsoft.Extensions.DependencyInjection;

# 定义命名空间
namespace MicaSetup.Design.Controls;

# 禁用特定的编译器警告
#pragma warning disable CS8618

# 定义 HostBuilder 类，实现 IHostBuilder 接口
public class HostBuilder : IHostBuilder
{
    # 定义一个 IApp 类型的属性
    public IApp App { get; set; }
    # 定义一个 ServiceProvider 类型的属性
    public ServiceProvider ServiceProvider { get; set; }

    # 实现 CreateApp 方法，初始化 App 属性并返回当前对象
    public IHostBuilder CreateApp()
    {
        # 创建新的 App 实例并赋值给 App 属性
        App = new App();
        # 返回当前 HostBuilder 对象
        return this;
    }

    # 实现 RunApp 方法，运行 App
    public void RunApp()
    {
        # 如果 App 不为 null，则调用其 Run 方法
        App?.Run();
    }
}
```