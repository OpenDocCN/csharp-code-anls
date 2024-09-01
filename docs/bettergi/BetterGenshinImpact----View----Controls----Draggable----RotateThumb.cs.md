# `.\better-genshin-impact\BetterGenshinImpact\View\Controls\Draggable\RotateThumb.cs`

```cs
# 引入系统、WPF 控件以及相关命名空间
﻿using System;
using System.Windows;
using System.Windows.Controls;
using System.Windows.Controls.Primitives;
using System.Windows.Input;
using System.Windows.Media;

namespace BetterGenshinImpact.View.Controls;

# 定义一个旋转操作的 Thumb 控件，继承自 Thumb
public class RotateThumb : Thumb
{
    # 初始角度
    private double initialAngle;
    # 旋转变换对象
    private RotateTransform? rotateTransform;
    # 起始向量
    private Vector startVector;
    # 控件中心点
    private Point centerPoint;
    # 需要旋转的内容控件
    private ContentControl? designerItem;
    # 包含内容控件的画布
    private Canvas? canvas;

    # 构造函数，注册拖动事件处理程序
    public RotateThumb()
    {
        DragDelta += OnRotateThumbDragDelta;  # 注册拖动中事件处理程序
        DragStarted += OnRotateThumbDragStarted;  # 注册拖动开始事件处理程序
    }

    # 拖动开始事件处理程序
    private void OnRotateThumbDragStarted(object sender, DragStartedEventArgs e)
    {
        # 从数据上下文获取内容控件
        designerItem = DataContext as ContentControl;

        if (designerItem is not null)
        {
            # 获取内容控件的父画布
            canvas = VisualTreeHelper.GetParent(designerItem) as Canvas;

            if (canvas is not null)
            {
                # 计算内容控件的中心点在画布上的位置
                centerPoint = designerItem.TranslatePoint(
                    new Point(designerItem.Width * designerItem.RenderTransformOrigin.X,
                              designerItem.Height * designerItem.RenderTransformOrigin.Y),
                              canvas);

                # 获取鼠标在画布上的起始位置
                Point startPoint = Mouse.GetPosition(canvas);
                # 计算鼠标起始位置与中心点的向量
                startVector = Point.Subtract(startPoint, centerPoint);

                # 获取当前内容控件的旋转变换对象，如果不存在则创建
                rotateTransform = designerItem.RenderTransform as RotateTransform;
                if (rotateTransform is null)
                {
                    designerItem.RenderTransform = new RotateTransform(0);  # 设置初始旋转角度为 0
                    initialAngle = 0;  # 记录初始角度
                }
                else
                {
                    initialAngle = rotateTransform.Angle;  # 记录当前角度
                }
            }
        }
    }

    # 拖动过程中事件处理程序
    private void OnRotateThumbDragDelta(object sender, DragDeltaEventArgs e)
    {
        if (designerItem is not null && canvas is not null)
        {
            # 获取鼠标在画布上的当前位置
            Point currentPoint = Mouse.GetPosition(canvas);
            # 计算当前鼠标位置与中心点的向量
            Vector deltaVector = Point.Subtract(currentPoint, centerPoint);

            # 计算起始向量与当前向量之间的角度
            double angle = Vector.AngleBetween(startVector, deltaVector);

            # 获取旋转变换对象，并更新其角度
            RotateTransform? rotateTransform = designerItem.RenderTransform as RotateTransform;
            rotateTransform!.Angle = initialAngle + Math.Round(angle, 0);
            # 使内容控件重新测量
            designerItem.InvalidateMeasure();
        }
    }
}
```