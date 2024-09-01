# `.\better-genshin-impact\BetterGenshinImpact\Model\MaskButton.cs`

```cs
# 引入相关命名空间
﻿using BetterGenshinImpact.GameTask;
using CommunityToolkit.Mvvm.Input;
using OpenCvSharp;
using System;

namespace BetterGenshinImpact.Model
{
    # 定义 MaskButton 类
    public class MaskButton
    {
        # 定义只读属性 Name
        public string Name { get; }
        # 定义可读写属性 X
        public double X { get; set; }
        # 定义可读写属性 Y
        public double Y { get; set; }
        # 定义可读写属性 Width
        public double Width { get; set; }
        # 定义可读写属性 Height
        public double Height { get; set; }
        # 定义可读写属性 ClickAction，类型为 IRelayCommand
        public IRelayCommand ClickAction { get; set; }

        # 构造函数，初始化 MaskButton 对象
        public MaskButton(string name, Rect rect, Action clickAction)
        {
            # 设置 Name 属性
            Name = name;
            # 获取当前 DPI 缩放比例
            var scale = TaskContext.Instance().DpiScale;
            # 按照 DPI 缩放比例调整 X 坐标
            X = rect.X / scale;
            # 按照 DPI 缩放比例调整 Y 坐标
            Y = rect.Y / scale;
            # 按照 DPI 缩放比例调整 Width
            Width = rect.Width / scale;
            # 按照 DPI 缩放比例调整 Height
            Height = rect.Height / scale;
            # 将点击操作封装成 RelayCommand 并赋值给 ClickAction 属性
            ClickAction = new RelayCommand(clickAction);
        }

        # 重写 Equals 方法以比较两个 MaskButton 对象是否相等
        public override bool Equals(object? obj)
        {
            # 检查 obj 是否为 MaskButton 类型
            if (obj is MaskButton button)
            {
                # 比较 Name 属性是否相等
                return Name == button.Name;
            }

            # 如果 obj 不是 MaskButton 类型，则返回 false
            return false;
        }

        # 重写 GetHashCode 方法以返回 Name 的哈希码
        public override int GetHashCode()
        {
            # 返回 Name 属性的哈希码
            return Name.GetHashCode();
        }
    }
}
```