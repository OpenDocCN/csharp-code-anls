# `.\better-genshin-impact\BetterGenshinImpact\GameTask\AutoSkip\Model\HangoutOption.cs`

```cs
﻿using BetterGenshinImpact.GameTask.Common; // 引入通用功能模块
using BetterGenshinImpact.GameTask.Model.Area; // 引入区域模型
using Microsoft.Extensions.Logging; // 引入日志记录功能
using OpenCvSharp; // 引入 OpenCV 库
using System; // 引入系统功能

namespace BetterGenshinImpact.GameTask.AutoSkip.Model; // 定义命名空间

public class HangoutOption : IDisposable // 定义 HangoutOption 类，支持资源释放
{
    public Region IconRect { get; set; } // 图标区域属性

    public ImageRegion? TextRect { get; set; } // 文字区域属性，可为空

    public bool IsSelected { get; set; } // 是否被选中属性

    public string OptionTextSrc { get; set; } = ""; // 选项文字来源，默认为空字符串

    public HangoutOption(Region iconRect, bool selected) // 构造函数，初始化图标区域和选中状态
    {
        IconRect = iconRect; // 设置图标区域
        IsSelected = selected; // 设置选中状态

        // 选项文字所在区域初始化
        // 选项图标往上下区域扩展 2/3
        var r = Rect.Empty; // 初始化矩形区域为空
        var captureArea = TaskContext.Instance().SystemInfo.ScaleMax1080PCaptureRect; // 获取捕获区域
        var assetScale = TaskContext.Instance().SystemInfo.AssetScale; // 获取资产缩放比例
        if (IconRect.Left > captureArea.Width / 2) // 判断图标是否在右半边
        {
            // 右边的选项
            r = new Rect(IconRect.Right, IconRect.Top - IconRect.Height * 2 / 3, captureArea.Width - IconRect.Right - (int)(10 * assetScale), IconRect.Height + IconRect.Height * 4 / 3); // 计算右侧选项的文字区域
        }
        else if (IconRect.Right < captureArea.Width / 2) // 判断图标是否在左半边
        {
            // 左边的选项
            r = new Rect((int)(10 * assetScale), IconRect.Top - IconRect.Height * 2 / 3, IconRect.Left - (int)(10 * assetScale), IconRect.Height + IconRect.Height * 4 / 3); // 计算左侧选项的文字区域
        }
        else
        {
            TaskControl.Logger.LogError("自动邀约：识别到错误位置的选项图标 {Rect}", IconRect); // 记录错误日志
        }

        if (r.Width < captureArea.Width / 8) // 检查文字区域宽度是否过小
        {
            TaskControl.Logger.LogError("自动邀约：选项文字区域过小 {Rect}", TextRect); // 记录错误日志
            r = Rect.Empty; // 将文字区域设置为空
        }

        if (r != Rect.Empty) // 如果文字区域不为空
        {
            if (iconRect.Prev is ImageRegion prev) // 检查前一个区域是否为 ImageRegion
            {
                TextRect = prev.DeriveCrop(r); // 从前一个区域衍生出文字区域
            }
            else
            {
                throw new Exception("HangoutOption: IconRect.Prev is not ImageRegion"); // 抛出异常
            }
        }
    }

    public void Move() // 移动图标区域
    {
        IconRect.Move(); // 调用图标区域的移动方法
    }

    public void Click() // 点击图标区域
    {
        IconRect.Click(); // 调用图标区域的点击方法
    }

    public void BackgroundClick() // 背景点击图标区域
    {
        IconRect.BackgroundClick(); // 调用图标区域的背景点击方法
    }

    public void Dispose() // 释放资源
    {
        IconRect.Dispose(); // 释放图标区域资源
        TextRect?.Dispose(); // 释放文字区域资源（如果存在）
    }
}
```