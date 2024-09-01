# `.\better-genshin-impact\BetterGenshinImpact\Core\Recognition\RecognitionTypes.cs`

```cs
# 定义命名空间 BetterGenshinImpact.Core.Recognition
﻿namespace BetterGenshinImpact.Core.Recognition;

# 定义一个枚举类型 RecognitionTypes
public enum RecognitionTypes
{
    # 默认值，表示没有匹配类型
    None,
    # 使用模板匹配算法进行识别
    TemplateMatch, // 模板匹配
    # 使用颜色匹配算法进行识别
    ColorMatch, // 颜色匹配
    # 使用光学字符识别 (OCR) 技术进行文字识别并匹配
    OcrMatch, // 文字识别并匹配
    # 仅使用光学字符识别 (OCR) 技术进行文字识别
    Ocr, // 仅文字识别
    # 提取指定颜色后使用 OCR 进行文字识别
    ColorRangeAndOcr, // 提取指定颜色后进行文字识别
    # 进行目标检测
    Detect
}
```