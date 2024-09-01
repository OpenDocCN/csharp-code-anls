# `.\better-genshin-impact\BetterGenshinImpact\View\Controls\Draggable\MoveThumb.cs`

```cs
# 引入所需的命名空间
﻿using System.Windows;
using System.Windows.Controls;
using System.Windows.Controls.Primitives;
using System.Windows.Media;

namespace BetterGenshinImpact.View.Controls;

# 定义一个继承自 Thumb 的 MoveThumb 类
public class MoveThumb : Thumb
{
    # 定义 RotateTransform 和 ContentControl 类型的私有字段
    private RotateTransform? rotateTransform;
    private ContentControl? designerItem;

    # MoveThumb 类的构造函数
    public MoveThumb()
    {
        # 订阅 DragStarted 事件的处理函数
        DragStarted += OnMoveThumbDragStarted;
        # 订阅 DragDelta 事件的处理函数
        DragDelta += OnMoveThumbDragDelta;
    }

    # 处理 DragStarted 事件的方法
    private void OnMoveThumbDragStarted(object sender, DragStartedEventArgs e)
    {
        # 将 DataContext 转换为 ContentControl 类型，并赋值给 designerItem
        designerItem = DataContext as ContentControl;

        # 如果 designerItem 不为 null，则获取其 RotateTransform
        if (designerItem != null)
        {
            rotateTransform = designerItem.RenderTransform as RotateTransform;
        }
    }

    # 处理 DragDelta 事件的方法
    private void OnMoveThumbDragDelta(object sender, DragDeltaEventArgs e)
    {
        # 如果 designerItem 不为 null
        if (designerItem is not null)
        {
            # 创建一个表示拖拽变化的点
            Point dragDelta = new(e.HorizontalChange, e.VerticalChange);

            # 如果 rotateTransform 不为 null，应用其变换到拖拽变化点
            if (rotateTransform is not null)
            {
                dragDelta = rotateTransform.Transform(dragDelta);
            }

            # 更新 designerItem 的水平位置
            Canvas.SetLeft(designerItem, Canvas.GetLeft(designerItem) + dragDelta.X);
            # 更新 designerItem 的垂直位置
            Canvas.SetTop(designerItem, Canvas.GetTop(designerItem) + dragDelta.Y);
        }
    }
}
```