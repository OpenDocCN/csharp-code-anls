# `.\better-genshin-impact\BetterGenshinImpact\Core\Recognition\OCR\OcrFactory.cs`

```cs
﻿using System;  // 引入 System 命名空间，以便使用系统基础功能

namespace BetterGenshinImpact.Core.Recognition.OCR;  // 定义命名空间 BetterGenshinImpact.Core.Recognition.OCR

public class OcrFactory  // 定义公共类 OcrFactory
{
    // public static IOcrService Media = Create(OcrEngineTypes.Media);  // 注释掉的代码行，用于创建 Media 类型的 OCR 服务

    public static IOcrService Paddle { get; } = Create(OcrEngineTypes.Paddle);  // 定义公共静态属性 Paddle，使用 Create 方法创建 Paddle 类型的 OCR 服务

    public static IOcrService Create(OcrEngineTypes type)  // 定义公共静态方法 Create，接受一个 OcrEngineTypes 类型的参数，返回一个 IOcrService 类型的实例
    {
        return type switch  // 使用 switch 表达式根据传入的类型选择返回不同的 OCR 服务实例
        {
            OcrEngineTypes.Paddle => new PaddleOcrService(),  // 如果类型是 Paddle，则创建并返回 PaddleOcrService 实例
            _ => throw new ArgumentOutOfRangeException(nameof(type), type, null),  // 如果类型不在预期范围内，抛出 ArgumentOutOfRangeException 异常
        };
    }
}
```