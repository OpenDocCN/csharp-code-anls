# `.\better-genshin-impact\Fischless.HotkeyCapture\SystemErrorCodes.cs`

```cs
# 定义命名空间 Fischless.HotkeyCapture
﻿namespace Fischless.HotkeyCapture;

# 声明一个内部封闭的类 SystemErrorCodes
internal sealed class SystemErrorCodes
{
    # 定义一个常量，表示热键已经被注册的错误码
    public const int ERROR_HOTKEY_ALREADY_REGISTERED = 0x581;
    # 定义一个常量，表示热键未被注册的错误码
    public const int ERROR_HOTKEY_NOT_REGISTERED = 0x58B;
}
```