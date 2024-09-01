# `.\better-genshin-impact\Vision.WindowCapture\CaptureModeExtensions.cs`

```cs
# 定义在 Vision.WindowCapture 命名空间中的静态类 CaptureModeExtensions
﻿namespace Vision.WindowCapture;

# 定义静态类 CaptureModeExtensions
public static class CaptureModeExtensions
{
    # 定义一个扩展方法，将字符串转换为 CaptureModeEnum 类型
    public static CaptureModeEnum ToCaptureMode(this string modeName)
    {
        # 将字符串 modeName 解析为 CaptureModeEnum 枚举值
        return (CaptureModeEnum)Enum.Parse(typeof(CaptureModeEnum), modeName);
    }
}
```