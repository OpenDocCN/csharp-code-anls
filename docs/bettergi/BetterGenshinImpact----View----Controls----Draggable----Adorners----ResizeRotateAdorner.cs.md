# `.\better-genshin-impact\BetterGenshinImpact\View\Controls\Draggable\Adorners\ResizeRotateAdorner.cs`

```cs
# 引入所需的 WPF 命名空间
﻿using System.Windows;
using System.Windows.Controls;
using System.Windows.Documents;
using System.Windows.Media;

# 定义命名空间
namespace BetterGenshinImpact.View.Controls.Adorners;

# 定义一个类 ResizeRotateAdorner，继承自 Adorner
public class ResizeRotateAdorner : Adorner
{
    # 定义一个只读的 VisualCollection，用于存储视觉子元素
    private readonly VisualCollection visuals;
    # 定义一个只读的 ResizeRotateChrome 实例，用于实现调整大小和旋转功能
    private readonly ResizeRotateChrome chrome;

    # 重写 VisualChildrenCount 属性，用于返回视觉子元素的数量
    protected override int VisualChildrenCount
    {
        get
        {
            return visuals.Count; # 返回 visuals 集合中的元素数量
        }
    }

    # 构造函数，接收一个 ContentControl 参数并调用基类构造函数
    public ResizeRotateAdorner(ContentControl? designerItem)
        : base(designerItem)
    {
        SnapsToDevicePixels = true; # 设置该 Adorner 对象是否应将其布局对齐到设备的像素网格
        # 初始化 ResizeRotateChrome 实例，并将其 DataContext 设置为 designerItem
        chrome = new ResizeRotateChrome
        {
            DataContext = designerItem
        };
        # 创建 VisualCollection 实例并将 chrome 添加到其中
        visuals = new VisualCollection(this)
        {
            chrome
        };
    }

    # 重写 ArrangeOverride 方法，用于确定子元素的布局
    protected override Size ArrangeOverride(Size arrangeBounds)
    {
        # 安排 chrome 的位置和大小
        chrome.Arrange(new Rect(arrangeBounds));
        return arrangeBounds; # 返回安排后的大小
    }

    # 重写 GetVisualChild 方法，用于获取指定索引的视觉子元素
    protected override Visual GetVisualChild(int index)
    {
        return visuals[index]; # 返回指定索引的视觉子元素
    }
}
```