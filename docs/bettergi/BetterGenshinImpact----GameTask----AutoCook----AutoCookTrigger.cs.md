# `.\better-genshin-impact\BetterGenshinImpact\GameTask\AutoCook\AutoCookTrigger.cs`

```cs
﻿using BetterGenshinImpact.Core.Recognition.OpenCv; // 引入用于图像识别的 OpenCV 库
using BetterGenshinImpact.GameTask.Common.Element.Assets; // 引入游戏任务中的元素资产
using Microsoft.Extensions.Logging; // 引入日志记录功能
using OpenCvSharp; // 引入 OpenCV 的 .NET 封装库
using System.Linq; // 引入 LINQ 扩展方法

namespace BetterGenshinImpact.GameTask.AutoCook; // 定义命名空间

public class AutoCookTrigger : ITaskTrigger // 定义 AutoCookTrigger 类实现 ITaskTrigger 接口
{
    private readonly ILogger<AutoCookTrigger> _logger = App.GetLogger<AutoCookTrigger>(); // 创建用于记录日志的实例

    public string Name => "自动烹饪"; // 任务名称属性
    public bool IsEnabled { get; set; } // 是否启用的属性
    public int Priority => 50; // 任务优先级属性
    public bool IsExclusive { get; set; } // 是否独占的属性

    public void Init() // 初始化方法
    {
        IsEnabled = TaskContext.Instance().Config.AutoCookConfig.Enabled; // 根据配置初始化是否启用
        IsExclusive = false; // 设置为非独占
    }

    public void OnCapture(CaptureContent content) // 捕获内容时调用的方法
    {
        // 判断是否处于烹饪界面
        IsExclusive = false; // 设置为非独占状态
        content.CaptureRectArea.Find(ElementAssets.Instance.UiLeftTopCookIcon, _ => // 查找烹饪图标
        {
            IsExclusive = true; // 设置为独占状态
            var captureRect = TaskContext.Instance().SystemInfo.ScaleMax1080PCaptureRect; // 获取截图区域
            using var region = content.CaptureRectArea.DeriveCrop(0, captureRect.Height / 2, captureRect.Width, captureRect.Height / 2); // 从捕获区域中裁剪一部分区域
            var perfectBarRects = ContoursHelper.FindSpecifyColorRects(region.SrcMat, new Scalar(255, 192, 64), 0, 8); // 查找特定颜色的矩形区域
            if (perfectBarRects.Count >= 2) // 如果找到的矩形区域数量大于等于2
            {
                // 点击烹饪按钮
                var btnList = ContoursHelper.FindSpecifyColorRects(region.SrcMat, new Scalar(255, 255, 192), 12, 12); // 查找另一种颜色的矩形区域
                if (btnList.Count >= 1) // 如果找到的按钮数量大于等于1
                {
                    if (btnList.Count > 1) // 如果找到的按钮数量大于1
                    {
                        _logger.LogWarning("自动烹饪：{Text}", "识别到多个结束烹饪按钮"); // 记录警告日志
                        btnList = [.. btnList.OrderByDescending(r => r.Width)]; // 根据按钮宽度降序排列
                    }
                    var btn = btnList[0]; // 选择最宽的按钮
                    var x = btn.X + btn.Width / 2; // 计算按钮中心的 X 坐标
                    var y = btn.Y + btn.Height / 2; // 计算按钮中心的 Y 坐标
                    region.ClickTo(x, y); // 点击按钮的中心位置
                    _logger.LogInformation("自动烹饪：{Text}", "点击结束按钮"); // 记录信息日志
                }
            }
        });
    }
}
```