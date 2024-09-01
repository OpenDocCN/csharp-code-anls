# `.\better-genshin-impact\BetterGenshinImpact\GameTask\AutoFishing\Model\OneFish.cs`

```cs
# 引入 OpenCvSharp 库，用于图像处理
﻿using OpenCvSharp;

# 定义命名空间，组织代码结构
namespace BetterGenshinImpact.GameTask.AutoFishing.Model
{
    # 定义一个表示鱼类的类
    public class OneFish
    {
        # 定义一个属性，用于存储鱼的类型
        public BigFishType FishType { get; set; }

        # 定义一个属性，用于存储鱼的矩形区域
        public Rect Rect { get; set; }

        # 定义一个属性，用于存储对鱼的置信度
        public float Confidence { get; set; }

        # 定义构造函数，用于初始化 OneFish 实例
        public OneFish(string name, Rect rect, float confidence)
        {
            # 通过名称创建鱼的类型，并赋值给 FishType 属性
            FishType = BigFishType.FromName(name);
            # 赋值矩形区域到 Rect 属性
            Rect = rect;
            # 赋值置信度到 Confidence 属性
            Confidence = confidence;
        }
    }
}
```