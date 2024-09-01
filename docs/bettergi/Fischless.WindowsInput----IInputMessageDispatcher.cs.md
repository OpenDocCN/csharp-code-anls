# `.\better-genshin-impact\Fischless.WindowsInput\IInputMessageDispatcher.cs`

```cs
# 引入 Vanara.PInvoke 命名空间，提供 PInvoke 方法的封装
﻿using Vanara.PInvoke;

# 定义 Fischless.WindowsInput 命名空间，包含输入相关的接口
namespace Fischless.WindowsInput;

# 声明内部接口 IInputMessageDispatcher，限定于当前程序集使用
internal interface IInputMessageDispatcher
{
    # 定义一个方法 DispatchInput，用于处理用户输入
    public void DispatchInput(User32.INPUT[] inputs);
}
```