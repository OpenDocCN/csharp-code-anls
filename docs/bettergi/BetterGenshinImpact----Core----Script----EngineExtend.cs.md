# `.\better-genshin-impact\BetterGenshinImpact\Core\Script\EngineExtend.cs`

```cs
﻿using BetterGenshinImpact.Core.Script.Dependence;  // 引入 BetterGenshinImpact.Core.Script.Dependence 命名空间
using BetterGenshinImpact.Core.Script.Dependence.Model;  // 引入 BetterGenshinImpact.Core.Script.Dependence.Model 命名空间
using Microsoft.ClearScript;  // 引入 Microsoft.ClearScript 命名空间
using System.Threading.Tasks;  // 引入 System.Threading.Tasks 命名空间

namespace BetterGenshinImpact.Core.Script;  // 定义命名空间 BetterGenshinImpact.Core.Script

public class EngineExtend  // 定义公开类 EngineExtend
{
    public static void InitHost(IScriptEngine engine, string workDir, string[]? searchPaths = null)  // 定义公开静态方法 InitHost，初始化脚本引擎
    {
        // engine.AddHostObject("xHost", new ExtendedHostFunctions());  // 有越权的安全风险  // 被注释掉的代码：将 ExtendedHostFunctions 实例添加为脚本引擎的主机对象，可能存在安全风险

        // 添加我的自定义实例化对象  // 注释：添加自定义的主机对象以供脚本使用
        engine.AddHostObject("keyMouseScript", new KeyMouseScript(workDir));  // 将 KeyMouseScript 实例添加为脚本引擎的主机对象，提供键盘和鼠标控制功能
        engine.AddHostObject("autoPathing", new AutoPathing(workDir));  // 将 AutoPathing 实例添加为脚本引擎的主机对象，提供自动路径规划功能
        engine.AddHostObject("genshin", new Dependence.Genshin());  // 将 Genshin 实例添加为脚本引擎的主机对象，提供与 Genshin 相关的功能
        engine.AddHostObject("log", new Log());  // 将 Log 实例添加为脚本引擎的主机对象，提供日志记录功能
        engine.AddHostObject("file", new LimitedFile(workDir));  // 将 LimitedFile 实例添加为脚本引擎的主机对象，限制文件访问以增强安全性

        // 实时任务调度器  // 注释：添加用于实时任务调度的对象
        engine.AddHostObject("dispatcher", new Dispatcher());  // 将 Dispatcher 实例添加为脚本引擎的主机对象，提供任务调度功能
        engine.AddHostType("RealtimeTimer", typeof(RealtimeTimer));  // 将 RealtimeTimer 类型添加为脚本引擎的主机类型，允许脚本使用实时定时器

        // 直接添加方法
#pragma warning disable CS8974 // Converting method group to non-delegate type  // 禁用警告 CS8974：方法组转换为非委托类型
        engine.AddHostObject("sleep", GlobalMethod.Sleep);  // 将 GlobalMethod.Sleep 方法添加为脚本引擎的主机对象，提供休眠功能
#pragma warning restore CS8974 // Converting method group to non-delegate type  // 恢复警告 CS8974

        // 添加C#的类型  // 注释：将 C# 类型添加到脚本引擎中
        engine.AddHostType(typeof(Task));  // 将 Task 类型添加为脚本引擎的主机类型，允许脚本使用异步任务

        // 导入 CommonJS 模块  // 注释：配置文档设置以允许 CommonJS 模块的导入
        // https://microsoft.github.io/ClearScript/2023/01/24/module-interop.html  // 链接：ClearScript 文档
        // https://github.com/microsoft/ClearScript/blob/master/ClearScriptTest/V8ModuleTest.cs  // 链接：ClearScript 测试示例
        engine.DocumentSettings.AccessFlags = DocumentAccessFlags.EnableFileLoading | DocumentAccessFlags.AllowCategoryMismatch;  // 设置文档访问标志以启用文件加载和允许类别不匹配
        if (searchPaths != null)  // 如果提供了搜索路径
        {
            engine.DocumentSettings.SearchPath = string.Join(';', searchPaths);  // 设置文档的搜索路径，以分号分隔
        }
    }
}
```