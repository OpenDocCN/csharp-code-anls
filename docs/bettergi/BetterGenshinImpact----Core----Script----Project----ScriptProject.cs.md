# `.\better-genshin-impact\BetterGenshinImpact\Core\Script\Project\ScriptProject.cs`

```cs
﻿using BetterGenshinImpact.Core.Config; // 引入 BetterGenshinImpact 核心配置命名空间
using Microsoft.ClearScript; // 引入 Microsoft ClearScript 相关库
using Microsoft.ClearScript.V8; // 引入 Microsoft ClearScript V8 引擎
using System; // 引入系统命名空间
using System.Diagnostics; // 引入调试命名空间
using System.IO; // 引入输入输出命名空间
using System.Threading.Tasks; // 引入异步任务命名空间
using System.Windows.Controls; // 引入 WPF 控件命名空间

namespace BetterGenshinImpact.Core.Script.Project // 定义命名空间 BetterGenshinImpact.Core.Script.Project
{
    public class ScriptProject // 定义 ScriptProject 类
    {
        public string ProjectPath { get; set; } // 项目路径属性
        public string ManifestFile { get; set; } // 清单文件路径属性

        public Manifest Manifest { get; set; } // Manifest 对象属性

        public string FolderName { get; set; } // 文件夹名称属性

        public ScriptProject(string folderName) // 构造函数，初始化 ScriptProject 实例
        {
            FolderName = folderName; // 设置文件夹名称
            ProjectPath = Path.Combine(Global.ScriptPath(), folderName); // 组合并设置项目路径
            ManifestFile = Path.GetFullPath(Path.Combine(ProjectPath, "manifest.json")); // 计算并设置清单文件的完整路径
            if (!File.Exists(ManifestFile)) // 如果清单文件不存在
            {
                throw new FileNotFoundException("manifest.json file not found."); // 抛出文件未找到异常
            }

            Manifest = Manifest.FromJson(File.ReadAllText(ManifestFile)); // 从清单文件加载 Manifest 对象
            Manifest.Validate(ProjectPath); // 验证 Manifest 对象
        }

        public StackPanel? LoadSettingUi(dynamic context) // 加载设置用户界面的函数
        {
            var settingItems = Manifest.LoadSettingItems(ProjectPath); // 从 Manifest 加载设置项
            if (settingItems.Count == 0) // 如果没有设置项
            {
                return null; // 返回 null
            }
            var stackPanel = new StackPanel(); // 创建一个新的 StackPanel
            foreach (var item in settingItems) // 遍历每个设置项
            {
                var controls = item.ToControl(context); // 将设置项转换为控件
                foreach (var control in controls) // 遍历每个控件
                {
                    stackPanel.Children.Add(control); // 将控件添加到 StackPanel 中
                }
            }

            return stackPanel; // 返回构建好的 StackPanel
        }

        public IScriptEngine BuildScriptEngine() // 构建脚本引擎的方法
        {
            IScriptEngine engine = new V8ScriptEngine(V8ScriptEngineFlags.UseCaseInsensitiveMemberBinding | V8ScriptEngineFlags.EnableTaskPromiseConversion); // 创建一个新的 V8 脚本引擎实例
            EngineExtend.InitHost(engine, ProjectPath, Manifest.Library); // 初始化脚本引擎主机
            return engine; // 返回脚本引擎实例
        }

        public async Task ExecuteAsync(dynamic? context = null) // 异步执行脚本的方法
        {
            var code = await LoadCode(); // 异步加载脚本代码
            var engine = BuildScriptEngine(); // 构建脚本引擎
            if (context != null) // 如果上下文不为 null
            {
                // 写入配置的内容
                engine.AddHostObject("settings", context); // 将上下文对象添加到脚本引擎中
            }
            try
            {
                await (Task)engine.Evaluate(code); // 异步执行脚本代码
            }
            catch (Exception e) // 捕捉异常
            {
                Debug.WriteLine(e); // 打印异常信息到调试输出
                throw; // 重新抛出异常
            }
            finally
            {
                engine.Dispose(); // 释放脚本引擎资源
            }
        }

        public async Task<string> LoadCode() // 异步加载脚本代码的方法
        {
            var code = await File.ReadAllTextAsync(Path.Combine(ProjectPath, Manifest.Main)); // 异步读取主脚本文件内容
            if (string.IsNullOrEmpty(code)) // 如果代码为空或 null
            {
                throw new FileNotFoundException("main js is empty."); // 抛出代码为空的异常
            }

            return code; // 返回脚本代码
        }
    }
}
```