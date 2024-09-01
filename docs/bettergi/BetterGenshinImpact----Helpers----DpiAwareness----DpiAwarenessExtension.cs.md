# `.\better-genshin-impact\BetterGenshinImpact\Helpers\DpiAwareness\DpiAwarenessExtension.cs`

```cs
# 使用 C# 的 Windows Presentation Foundation (WPF) 命名空间
﻿using System.Windows;

# 定义 BetterGenshinImpact.Helpers.DpiAwareness 命名空间
namespace BetterGenshinImpact.Helpers.DpiAwareness;

# 定义一个内部静态类 DpiAwarenessExtension
internal static class DpiAwarenessExtension
{
    # 定义一个扩展方法 InitializeDpiAwareness，为 Window 类型扩展功能
    public static void InitializeDpiAwareness(this Window window)
    {
        # 创建一个 DpiAwarenessController 对象，并传入当前的 Window 实例
        _ = new DpiAwarenessController(window);
    }
}
```