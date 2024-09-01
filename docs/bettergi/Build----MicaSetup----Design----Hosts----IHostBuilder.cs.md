# `.\better-genshin-impact\Build\MicaSetup\Design\Hosts\IHostBuilder.cs`

```cs
# 引入 Microsoft.Extensions.DependencyInjection 命名空间
﻿using Microsoft.Extensions.DependencyInjection;

# 定义命名空间 MicaSetup.Design.Controls
namespace MicaSetup.Design.Controls;

# 定义接口 IHostBuilder
public interface IHostBuilder
{
    # 定义属性 App，类型为 IApp，具有 getter 和 setter
    public IApp App { get; set; }
    # 定义属性 ServiceProvider，类型为 ServiceProvider，具有 getter 和 setter
    public ServiceProvider ServiceProvider { get; set; }

    # 定义方法 CreateApp，返回类型为 IHostBuilder
    public IHostBuilder CreateApp();

    # 定义方法 RunApp，无返回值
    public void RunApp();
}
```