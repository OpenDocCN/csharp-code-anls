# `.\better-genshin-impact\Vision.WindowCapture\WindowCaptureFactory.cs`

```cs
# 定义一个命名空间 Vision.WindowCapture
﻿namespace Vision.WindowCapture;

# 定义一个名为 WindowCaptureFactory 的公共类
public class WindowCaptureFactory
{
    # 定义一个公共的静态方法 ModeNames，返回一个字符串数组
    public static string[] ModeNames()
    {
        # 获取 CaptureModeEnum 枚举类型中所有名称的字符串数组
        return Enum.GetNames(typeof(CaptureModeEnum));
    }


    # 定义一个公共的静态方法 Create，接受一个 CaptureModeEnum 类型的参数，返回 IWindowCapture 类型
    public static IWindowCapture Create(CaptureModeEnum mode)
    {
        # 根据传入的 mode 参数选择创建不同的 IWindowCapture 实现
        return mode switch
        {
            # 如果 mode 是 CaptureModeEnum.BitBlt，创建并返回 BitBlt.BitBltCapture 实例
            CaptureModeEnum.BitBlt => new BitBlt.BitBltCapture(),
            # 如果 mode 是 CaptureModeEnum.WindowsGraphicsCapture，创建并返回 GraphicsCapture.GraphicsCapture 实例
            CaptureModeEnum.WindowsGraphicsCapture => new GraphicsCapture.GraphicsCapture(),
            # 如果 mode 不匹配以上情况，抛出 ArgumentOutOfRangeException 异常
            _ => throw new ArgumentOutOfRangeException(nameof(mode), mode, null),
        };
    }
}
```