# `.\better-genshin-impact\BetterGenshinImpact\ViewModel\MaskWindowViewModel.cs`

```cs
# 引入 BetterGenshinImpact 核心配置相关的命名空间
﻿using BetterGenshinImpact.Core.Config;

# 引入 BetterGenshinImpact 游戏任务相关的命名空间
using BetterGenshinImpact.GameTask;

# 引入 BetterGenshinImpact 辅助工具相关的命名空间
using BetterGenshinImpact.Helpers;

# 引入 BetterGenshinImpact 模型相关的命名空间
using BetterGenshinImpact.Model;

# 引入 BetterGenshinImpact 服务接口相关的命名空间
using BetterGenshinImpact.Service.Interface;

# 引入 CommunityToolkit MVVM 组件模型相关的命名空间
using CommunityToolkit.Mvvm.ComponentModel;

# 引入 CommunityToolkit MVVM 命令相关的命名空间
using CommunityToolkit.Mvvm.Input;

# 引入 CommunityToolkit MVVM 消息传递相关的命名空间
using CommunityToolkit.Mvvm.Messaging;

# 引入 CommunityToolkit MVVM 消息传递消息类型相关的命名空间
using CommunityToolkit.Mvvm.Messaging.Messages;

# 引入 PresentMonFps 命名空间（用于帧率监控）
using PresentMonFps;

# 引入 System.Collections.ObjectModel 命名空间（用于可观察集合）
using System.Collections.ObjectModel;

# 引入 System.Linq 命名空间（用于 LINQ 查询）
using System.Linq;

# 引入 System.Threading.Tasks 命名空间（用于异步编程）
using System.Threading.Tasks;

# 引入 System.Windows 命名空间（用于 Windows 窗口和控件）
using System.Windows;

# 引入 Vanara.PInvoke 命名空间（用于 Windows API 调用）
using Vanara.PInvoke;

# 声明命名空间 BetterGenshinImpact.ViewModel
namespace BetterGenshinImpact.ViewModel
{
    # 定义 MaskWindowViewModel 类，并继承自 ObservableRecipient 类，支持 MVVM 的消息传递和通知功能
    public partial class MaskWindowViewModel : ObservableRecipient
    {
    }
}
```