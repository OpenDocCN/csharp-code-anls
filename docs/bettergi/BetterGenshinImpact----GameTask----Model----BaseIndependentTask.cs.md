# `.\better-genshin-impact\BetterGenshinImpact\GameTask\Model\BaseIndependentTask.cs`

```cs
# 引入 Vanara.PInvoke 命名空间
﻿using Vanara.PInvoke;

# 定义命名空间 BetterGenshinImpact.GameTask.Model
namespace BetterGenshinImpact.GameTask.Model;

# 定义一个名为 BaseIndependentTask 的公共类
public class BaseIndependentTask
{
    # 定义一个受保护的只读属性 Info，该属性获取任务上下文实例的系统信息
    protected SystemInfo Info => TaskContext.Instance().SystemInfo;
    # 定义一个受保护的只读属性 CaptureRect，该属性获取任务上下文实例的系统信息中的捕获区域矩形
    protected RECT CaptureRect => TaskContext.Instance().SystemInfo.CaptureAreaRect;
    # 定义一个受保护的只读属性 AssetScale，该属性获取任务上下文实例的系统信息中的资产缩放比例
    protected double AssetScale => TaskContext.Instance().SystemInfo.AssetScale;
}
```