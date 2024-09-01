# `.\better-genshin-impact\Fischless.GameCapture\CaptureModeExtensions.cs`

```cs
# 定义一个命名空间 Fischless.GameCapture
﻿namespace Fischless.GameCapture;

# 定义一个静态类 CaptureModeExtensions
public static class CaptureModeExtensions
{
    # 定义一个扩展方法 ToCaptureMode，将 string 类型的 modeName 转换为 CaptureModes 枚举类型
    public static CaptureModes ToCaptureMode(this string modeName)
    {
        # 使用 Enum.Parse 方法将 modeName 字符串解析为 CaptureModes 枚举类型的值
        return (CaptureModes)Enum.Parse(typeof(CaptureModes), modeName);
    }
}
```