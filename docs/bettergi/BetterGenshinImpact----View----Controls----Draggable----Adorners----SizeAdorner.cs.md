# `.\better-genshin-impact\BetterGenshinImpact\View\Controls\Draggable\Adorners\SizeAdorner.cs`

```cs
# 引入所需的命名空间
﻿using System.Windows;
using System.Windows.Controls;
using System.Windows.Documents;
using System.Windows.Media;

# 定义名为 SizeAdorner 的类，继承自 Adorner 类
namespace BetterGenshinImpact.View.Controls.Adorners;

public class SizeAdorner : Adorner
{
    # 定义私有字段 chrome、visuals 和 designerItem
    private readonly SizeChrome chrome;
    private readonly VisualCollection visuals;
    private readonly ContentControl designerItem;

    # 重写 VisualChildrenCount 属性以返回视觉子项的数量
    protected override int VisualChildrenCount
    {
        get
        {
            return visuals.Count; # 返回 visuals 集合中的项数
        }
    }

    # 构造函数，初始化 SizeAdorner 实例
    public SizeAdorner(ContentControl designerItem)
        : base(designerItem)
    {
        SnapsToDevicePixels = true; # 设置是否将渲染对齐到设备像素
        this.designerItem = designerItem; # 初始化 designerItem 字段
        # 创建 SizeChrome 实例，并设置其 DataContext 为 designerItem
        chrome = new SizeChrome
        {
            DataContext = designerItem
        };
        # 创建 VisualCollection 实例并添加 chrome 到其中
        visuals = new VisualCollection(this)
        {
            chrome
        };
    }

    # 重写 GetVisualChild 方法以返回指定索引的视觉子项
    protected override Visual GetVisualChild(int index)
    {
        return visuals[index]; # 根据索引返回 visuals 集合中的视觉子项
    }

    # 重写 ArrangeOverride 方法以安排子项的布局
    protected override Size ArrangeOverride(Size arrangeBounds)
    {
        chrome.Arrange(new Rect(default, arrangeBounds)); # 安排 chrome 的位置和大小
        return arrangeBounds; # 返回布局边界大小
    }
}
```