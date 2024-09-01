# `.\better-genshin-impact\BetterGenshinImpact\Core\Recognition\OpenCv\Model\KeyPointFeatureBlock.cs`

```cs
﻿using OpenCvSharp;  // 引入 OpenCvSharp 库，用于图像处理相关功能
using System.Collections.Generic;  // 引入 System.Collections.Generic 命名空间，用于使用 List<T> 类型

namespace BetterGenshinImpact.Core.Recognition.OpenCv.Model;  // 定义命名空间

/// <summary>
/// 特征块
/// 对特征按图像区域进行划分
/// </summary>
public class KeyPointFeatureBlock
{
    // 存储特征点的列表
    public List<KeyPoint> KeyPointList { get; set; } = [];  // 初始化为空的列表

    private KeyPoint[]? keyPointArray;  // 存储特征点数组的私有字段，初始值为 null

    public KeyPoint[] KeyPointArray
    {
        get
        {
            // 如果 keyPointArray 为 null，则将 KeyPointList 转换为数组并赋值给 keyPointArray
            keyPointArray ??= [.. KeyPointList];
            // 返回特征点数组
            return keyPointArray;
        }
    }

    /// <summary>
    /// 在完整 KeyPoint[] 中的下标
    /// </summary>
    // 存储特征点在完整数组中的索引列表
    public List<int> KeyPointIndexList { get; set; } = [];  // 初始化为空的列表

    public Mat? Descriptor;  // 存储描述符的可选字段

    public int MergedCenterCellCol = -1;  // 合并中心单元格的列索引，初始值为 -1
    public int MergedCenterCellRow = -1;  // 合并中心单元格的行索引，初始值为 -1
}
```