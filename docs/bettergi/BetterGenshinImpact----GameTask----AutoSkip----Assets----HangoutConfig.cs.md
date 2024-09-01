# `.\better-genshin-impact\BetterGenshinImpact\GameTask\AutoSkip\Assets\HangoutConfig.cs`

```cs
﻿using BetterGenshinImpact.Core.Config; // 引入配置相关的命名空间
using BetterGenshinImpact.Model; // 引入模型相关的命名空间
using BetterGenshinImpact.Service; // 引入服务相关的命名空间
using System; // 引入系统命名空间，包含基本功能
using System.Collections.Generic; // 引入集合相关的命名空间
using System.IO; // 引入输入输出相关的命名空间
using System.Text.Json; // 引入 JSON 处理相关的命名空间

namespace BetterGenshinImpact.GameTask.AutoSkip.Assets; // 定义命名空间

public class HangoutConfig : Singleton<HangoutConfig> // 定义 HangoutConfig 类，继承自 Singleton<HangoutConfig>
{
    public Dictionary<string, List<string>> HangoutOptions; // 存储邀请分支选项的字典，键是选项名，值是选项列表
    public List<string> HangoutOptionsTitleList; // 存储所有选项名的列表

    private HangoutConfig() // 私有构造函数，防止外部实例化
    {
        // 邀约分支选项
        // 读取 hangout.json 文件的内容到字符串
        string hangoutJson = File.ReadAllText(Global.Absolute(@"GameTask\AutoSkip\Assets\hangout.json"));
        // 将 JSON 字符串反序列化为字典对象，如果反序列化失败，则抛出异常
        HangoutOptions = JsonSerializer.Deserialize<Dictionary<string, List<string>>>(hangoutJson,
            ConfigService.JsonOptions) ?? throw new Exception("hangout.json deserialize failed");
        // 初始化选项标题列表为字典键的列表
        HangoutOptionsTitleList = new List<string>(HangoutOptions.Keys);
    }
}
```