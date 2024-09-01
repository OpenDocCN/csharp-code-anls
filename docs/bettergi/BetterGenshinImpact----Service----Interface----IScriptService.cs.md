# `.\better-genshin-impact\BetterGenshinImpact\Service\Interface\IScriptService.cs`

```cs
# 引入 BetterGenshinImpact.Core.Script.Group 命名空间，包含脚本组相关的类型
﻿using BetterGenshinImpact.Core.Script.Group;

# 引入 System.Collections.Generic 命名空间，包含泛型集合类型
using System.Collections.Generic;

# 引入 System.Threading.Tasks 命名空间，包含异步编程相关的类型
using System.Threading.Tasks;

# 定义一个接口 IScriptService，用于提供脚本服务相关的方法
namespace BetterGenshinImpact.Service.Interface;

# 声明接口 IScriptService
public interface IScriptService
{
    # 定义一个异步方法 RunMulti，接收文件夹名称列表和可选的组名称
    Task RunMulti(List<string> folderNameList, string? groupName = null);

    # 定义一个异步方法 RunMulti，接收脚本组项目列表和组名称
    Task RunMulti(IEnumerable<ScriptGroupProject> projectList, string groupName);
}
```