# `.\better-genshin-impact\BetterGenshinImpact\Core\Recognition\OpenCv\FeatureMatch\Feature2DType.cs`

```cs
# 定义一个命名空间，组织有关 OpenCv 特征匹配的核心功能
﻿namespace BetterGenshinImpact.Core.Recognition.OpenCv.FeatureMatch;

# 定义一个枚举类型，用于表示不同的特征检测算法
public enum Feature2DType
{
    # 表示 SURF（加速鲁棒特征），用于特征检测和描述
    // ReSharper disable once InconsistentNaming
    SURF,

    # 表示 SIFT（尺度不变特征变换），用于特征检测和描述
    // ReSharper disable once InconsistentNaming
    SIFT
}
```