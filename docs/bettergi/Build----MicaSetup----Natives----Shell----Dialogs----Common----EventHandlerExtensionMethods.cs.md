# `.\better-genshin-impact\Build\MicaSetup\Natives\Shell\Dialogs\Common\EventHandlerExtensionMethods.cs`

```cs
# 引入系统命名空间
﻿using System;

# 定义 MicaSetup.Shell.Dialogs 命名空间
namespace MicaSetup.Shell.Dialogs;

# 定义公共静态类 EventHandlerExtensionMethods
public static class EventHandlerExtensionMethods
{
    # 扩展方法：安全地触发 EventHandler 事件
    public static void SafeRaise(this EventHandler eventHandler, object sender)
    {
        # 如果 eventHandler 不为空，则触发事件，传递 sender 和空的 EventArgs
        eventHandler?.Invoke(sender, EventArgs.Empty);
    }

    # 扩展方法：安全地触发泛型 EventHandler 事件
    public static void SafeRaise<T>(this EventHandler<T> eventHandler, object sender, T args) where T : EventArgs
    {
        # 如果 eventHandler 不为空，则触发事件，传递 sender 和指定的 EventArgs
        eventHandler?.Invoke(sender, args);
    }

    # 扩展方法：安全地触发 EventHandler<EventArgs> 事件，简化调用
    public static void SafeRaise(this EventHandler<EventArgs> eventHandler, object sender) => SafeRaise(eventHandler, sender, EventArgs.Empty);
}
```