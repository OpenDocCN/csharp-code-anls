# `.\better-genshin-impact\Fischless.GameCapture\GameCaptureFactory.cs`

```cs
# 定义命名空间 Fischless.GameCapture
﻿namespace Fischless.GameCapture;

# 定义 GameCaptureFactory 类
public class GameCaptureFactory
{
    # 定义静态方法 ModeNames，返回 CaptureModes 枚举的名称
    public static string[] ModeNames()
    {
        # 获取并返回 CaptureModes 枚举的所有名称
        return Enum.GetNames(typeof(CaptureModes));
    }

    # 定义静态方法 Create，根据 CaptureModes 枚举创建相应的 IGameCapture 实例
    public static IGameCapture Create(CaptureModes mode)
    {
        # 使用 switch 表达式，根据 mode 值返回不同的 IGameCapture 实例
        return mode switch
        {
            # 如果 mode 是 BitBlt，返回 BitBltCapture 实例
            CaptureModes.BitBlt => new BitBlt.BitBltCapture(),
            # 如果 mode 是 WindowsGraphicsCapture，返回 GraphicsCapture 实例
            CaptureModes.WindowsGraphicsCapture => new Graphics.GraphicsCapture(),
            # 如果 mode 是 DwmGetDxSharedSurface，返回 SharedSurfaceCapture 实例
            CaptureModes.DwmGetDxSharedSurface => new DwmSharedSurface.SharedSurfaceCapture(),
            # 如果 mode 不匹配上述值，抛出 ArgumentOutOfRangeException 异常
            _ => throw new ArgumentOutOfRangeException(nameof(mode), mode, null),
        };
    }
}
```