# `.\better-genshin-impact\BetterGenshinImpact\Core\Recognition\ONNX\SVTR\TextInferenceFactory.cs`

```cs
# 引入系统命名空间
﻿using System;

# 定义命名空间 BetterGenshinImpact.Core.Recognition.ONNX.SVTR
namespace BetterGenshinImpact.Core.Recognition.ONNX.SVTR;

# 定义 TextInferenceFactory 类
public class TextInferenceFactory
{
    # 定义一个静态属性 Pick，初始化为通过 Create 方法创建的实例，使用 OcrEngineTypes.YapModel 类型
    public static ITextInference Pick { get; } = Create(OcrEngineTypes.YapModel);

    # 定义静态方法 Create，根据传入的 OcrEngineTypes 类型创建对应的 ITextInference 实例
    public static ITextInference Create(OcrEngineTypes type)
    {
        # 根据 OcrEngineTypes 类型创建相应的 ITextInference 实例
        return type switch
        {
            # 如果类型是 YapModel，则返回 PickTextInference 的新实例
            OcrEngineTypes.YapModel => new PickTextInference(),
            # 如果类型不在预期范围内，抛出 ArgumentOutOfRangeException 异常
            _ => throw new ArgumentOutOfRangeException(nameof(type), type, null),
        };
    }
}
```