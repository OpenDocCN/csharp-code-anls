# `.\better-genshin-impact\Fischless.GameCapture\CaptureModes.cs`

```cs
# 定义一个命名空间 Fischless.GameCapture
﻿namespace Fischless.GameCapture;

# 定义一个公共枚举类型 CaptureModes
public enum CaptureModes
{
    # 使用 BitBlt 方法进行捕捉
    BitBlt,
    # 使用 Windows Graphics Capture API 进行捕捉
    WindowsGraphicsCapture,
    # 使用 DwmGetDxSharedSurface 方法进行捕捉
    DwmGetDxSharedSurface
}
```