# `.\better-genshin-impact\BetterGenshinImpact\Core\Recognition\OcrEngineTypes.cs`

```cs
# 定义命名空间 BetterGenshinImpact.Core.Recognition
﻿namespace BetterGenshinImpact.Core.Recognition;

# 定义一个枚举类型 OcrEngineTypes
public enum OcrEngineTypes
{
    # 注释掉的条目，用于通用 OCR 引擎
    //Media,
    # 通用的 Paddle OCR 引擎
    Paddle,

    # 特定模型 YasModel
    YasModel,

    # 特定模型 YapModel
    YapModel
}
```