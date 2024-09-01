# `.\better-genshin-impact\BetterGenshinImpact\GameTask\AutoSkip\Model\PaddleOcrResultRect.cs`

```cs
# 引入 OpenCvSharp 命名空间，用于处理图像处理相关功能
﻿using OpenCvSharp;

# 声明命名空间 BetterGenshinImpact.GameTask.AutoSkip.Model
namespace BetterGenshinImpact.GameTask.AutoSkip.Model;

# 定义一个记录结构体 PaddleOcrResultRect，包含三个字段
# 这个结构体用于表示 OCR 结果的矩形区域、识别的文本和评分
public record struct PaddleOcrResultRect(
    # 表示 OCR 结果的矩形区域
    Rect Rect, 
    # 表示 OCR 识别出的文本
    string Text, 
    # 表示 OCR 识别结果的评分
    float Score);
```