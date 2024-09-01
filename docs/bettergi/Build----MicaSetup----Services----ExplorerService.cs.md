# `.\better-genshin-impact\Build\MicaSetup\Services\ExplorerService.cs`

```cs
# 引入 MicaSetup.Helper 命名空间
﻿using MicaSetup.Helper;

# 定义 MicaSetup.Services 命名空间
namespace MicaSetup.Services;

# 定义 ExplorerService 类并实现 IExplorerService 接口
public class ExplorerService : IExplorerService
{
    # 实现接口中的 Refresh 方法
    public void Refresh()
    {
        # 调用 ExplorerHelper 类的 Refresh 方法
        ExplorerHelper.Refresh();
    }
}
```