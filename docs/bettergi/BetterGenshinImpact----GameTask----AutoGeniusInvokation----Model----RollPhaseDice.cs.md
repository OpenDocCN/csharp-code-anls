# `.\better-genshin-impact\BetterGenshinImpact\GameTask\AutoGeniusInvokation\Model\RollPhaseDice.cs`

```cs
# 引入必要的命名空间
﻿using System;
using System.Drawing;

# 定义命名空间
namespace BetterGenshinImpact.GameTask.AutoGeniusInvokation.Model
{
    # <summary>
    # 注释说明：投掷期间骰子
    # </summary>
    # 标记此类为过时，可能有更好的替代品
    [Obsolete]
    # 定义投掷期间骰子的类
    public class RollPhaseDice
    {
        # <summary>
        # 注释说明：元素类型
        # </summary>
        # 属性：表示骰子的元素类型
        public ElementalType Type { get; set; }

        # <summary>
        # 注释说明：中心点位置
        # </summary>
        # 属性：表示骰子的中心点位置
        public Point CenterPosition { get; set; }

        # 构造函数：初始化骰子的类型和中心点位置
        public RollPhaseDice(ElementalType type, Point centerPosition)
        {
            Type = type;
            CenterPosition = centerPosition;
        }

        # 默认构造函数：用于创建没有初始化属性的骰子对象
        public RollPhaseDice()
        {
        }

        # 重写 ToString 方法，以便返回对象的字符串表示
        public override string ToString()
        {
            # 返回骰子的类型和中心点位置的字符串表示
            return $"Type:{Type},CenterPosition:{CenterPosition}";
        }

        # 方法：模拟点击骰子位置的操作
        public void Click()
        {
            # 注释掉的代码：调用 MouseUtils.Click 方法，点击骰子的中心位置
            //MouseUtils.Click(CenterPosition.X, CenterPosition.Y);
        }
    }
}
```