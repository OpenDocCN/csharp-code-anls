# `.\better-genshin-impact\BetterGenshinImpact\ViewModel\Windows\AutoPickWhiteListViewModel.cs`

```cs
# 引入所需的命名空间
﻿using BetterGenshinImpact.Core.Config;
using BetterGenshinImpact.GameTask;
using System.Collections.Generic;
using System.Linq;
using System.Text.Json;

namespace BetterGenshinImpact.ViewModel.Windows;

# 定义 AutoPickWhiteListViewModel 类，继承自 FormViewModel<string>
public class AutoPickWhiteListViewModel : FormViewModel<string>
{
    # 构造函数
    public AutoPickWhiteListViewModel()
    {
        # 从指定路径读取 JSON 文件内容，如果文件存在
        var blackListJson = Global.ReadAllTextIfExist(@"User\pick_white_lists.json");
        # 如果读取的内容不为空
        if (!string.IsNullOrEmpty(blackListJson))
        {
            # 将 JSON 字符串反序列化为字符串列表，如果反序列化失败则初始化为空列表
            var blackList = JsonSerializer.Deserialize<List<string>>(blackListJson) ?? [];
            # 将反序列化得到的列表添加到当前对象中
            AddRange(blackList);
        }
    }

    # 重写 OnSave 方法
    public new void OnSave()
    {
        # 将当前对象的列表序列化为 JSON 字符串
        var blackListJson = JsonSerializer.Serialize(List.ToList());
        # 将序列化后的 JSON 字符串写入指定路径
        Global.WriteAllText(@"User\pick_white_lists.json", blackListJson);
        # 刷新游戏任务管理器的触发配置
        GameTaskManager.RefreshTriggerConfigs();
    }
}
```