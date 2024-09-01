# `.\better-genshin-impact\BetterGenshinImpact\View\Controls\Draggable\ResizeThumb.cs`

```cs
# 引入所
    {
        # 如果 designerItem 不为空
        if (designerItem is not null)
        {
            # 声明垂直和水平的增量变量
            double deltaVertical, deltaHorizontal;

            # 根据 VerticalAlignment 的不同处理垂直方向上的调整
            switch (VerticalAlignment)
            {
                # 如果对齐方式是底部
                case VerticalAlignment.Bottom:
                    # 计算垂直方向的增量，确保不低于最小高度
                    deltaVertical = Math.Min(-e.VerticalChange, designerItem.ActualHeight - designerItem.MinHeight);
                    # 调整 designerItem 的顶部位置和左边位置，并更新高度
                    Canvas.SetTop(designerItem, Canvas.GetTop(designerItem) + (transformOrigin.Y * deltaVertical * (1 - Math.Cos(-angle))));
                    Canvas.SetLeft(designerItem, Canvas.GetLeft(designerItem) - deltaVertical * transformOrigin.Y * Math.Sin(-angle));
                    designerItem.Height -= deltaVertical;
                    break;
                # 如果对齐方式是顶部
                case VerticalAlignment.Top:
                    # 计算垂直方向的增量，确保不低于最小高度
                    deltaVertical = Math.Min(e.VerticalChange, designerItem.ActualHeight - designerItem.MinHeight);
                    # 调整 designerItem 的顶部位置和左边位置，并更新高度
                    Canvas.SetTop(designerItem, Canvas.GetTop(designerItem) + deltaVertical * Math.Cos(-angle) + (transformOrigin.Y * deltaVertical * (1 - Math.Cos(-angle))));
                    Canvas.SetLeft(designerItem, Canvas.GetLeft(designerItem) + deltaVertical * Math.Sin(-angle) - (transformOrigin.Y * deltaVertical * Math.Sin(-angle)));
                    designerItem.Height -= deltaVertical;
                    break;
                default:
                    break;
            }

            # 根据 HorizontalAlignment 的不同处理水平方向上的调整
            switch (HorizontalAlignment)
            {
                # 如果对齐方式是左侧
                case HorizontalAlignment.Left:
                    # 计算水平方向的增量，确保不低于最小宽度
                    deltaHorizontal = Math.Min(e.HorizontalChange, designerItem.ActualWidth - designerItem.MinWidth);
                    # 调整 designerItem 的顶部位置和左边位置，并更新宽度
                    Canvas.SetTop(designerItem, Canvas.GetTop(designerItem) + deltaHorizontal * Math.Sin(angle) - transformOrigin.X * deltaHorizontal * Math.Sin(angle));
                    Canvas.SetLeft(designerItem, Canvas.GetLeft(designerItem) + deltaHorizontal * Math.Cos(angle) + (transformOrigin.X * deltaHorizontal * (1 - Math.Cos(angle))));
                    designerItem.Width -= deltaHorizontal;
                    break;
                # 如果对齐方式是右侧
                case HorizontalAlignment.Right:
                    # 计算水平方向的增量，确保不低于最小宽度
                    deltaHorizontal = Math.Min(-e.HorizontalChange, designerItem.ActualWidth - designerItem.MinWidth);
                    # 调整 designerItem 的顶部位置和左边位置，并更新宽度
                    Canvas.SetTop(designerItem, Canvas.GetTop(designerItem) - transformOrigin.X * deltaHorizontal * Math.Sin(angle));
                    Canvas.SetLeft(designerItem, Canvas.GetLeft(designerItem) + (deltaHorizontal * transformOrigin.X * (1 - Math.Cos(angle))));
                    designerItem.Width -= deltaHorizontal;
                    break;
                default:
                    break;
            }
        }

        # 处理事件标记为已处理
        e.Handled = true;
    }

    # 当拖拽操作完成时的处理方法
    private void OnResizeThumbDragCompleted(object sender, DragCompletedEventArgs e)
    {
        # 如果 adorner 不为空
        if (adorner is not null)
        {
            # 从画布的 AdornerLayer 中移除 adorner，并将 adorner 设为 null
            AdornerLayer.GetAdornerLayer(canvas)?.Remove(adorner);
            adorner = null;
        }
    }
这个代码段看起来是不完整的。请提供更多的上下文或代码，以便我可以为你添加合适的注释。
```