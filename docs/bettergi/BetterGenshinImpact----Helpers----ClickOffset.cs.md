# `.\better-genshin-impact\BetterGenshinImpact\Helpers\ClickOffset.cs`

```cs
﻿// 引用 BetterGenshinImpact.GameTask 命名空间中的类
// 引用 BetterGenshinImpact.Helpers.Extensions 命名空间中的扩展方法
// 引用 System 命名空间
//
// 定义 BetterGenshinImpact.Helpers 命名空间
//
// /// <summary>
// /// 不推荐使用
// /// 请使用 GameCaptureRegion.GameRegionClick 或 GameCaptureRegion.GameRegion1080PPosClick 替代
// /// </summary>
// [Obsolete] // 标记该类为过时，不推荐使用
// public class ClickOffset
// {
//     // 偏移量 X 属性
//     public int OffsetX { get; set; }
//     // 偏移量 Y 属性
//     public int OffsetY { get; set; }
//     // 资产缩放比例属性
//     public double AssetScale { get; set; }
//
//     // public double CaptureAreaScale { get; set; } // 捕获区域缩放比例属性（被注释掉）
//
//     // 默认构造函数
//     public ClickOffset()
//     {
//         // 检查 TaskContext 实例是否初始化
//         if (!TaskContext.Instance().IsInitialized)
//         {
//             // 如果未初始化，抛出异常
//             throw new Exception("请先启动");
//         }
//         // 获取捕获区域矩形
//         var captureArea = TaskContext.Instance().SystemInfo.CaptureAreaRect;
//         // 获取资产缩放比例
//         var assetScale = TaskContext.Instance().SystemInfo.AssetScale;
//         // 设置 OffsetX 属性
//         OffsetX = captureArea.X;
//         // 设置 OffsetY 属性
//         OffsetY = captureArea.Y;
//         // 设置 AssetScale 属性
//         AssetScale = assetScale;
//         // CaptureAreaScale = TaskContext.Instance().SystemInfo.CaptureAreaScale; // 捕获区域缩放比例（被注释掉）
//     }
//
//     // 带参数的构造函数
//     public ClickOffset(int offsetX, int offsetY, double assetScale)
//     {
//         // 检查 TaskContext 实例是否初始化
//         if (!TaskContext.Instance().IsInitialized)
//         {
//             // 如果未初始化，抛出异常
//             throw new Exception("请先启动");
//         }
//         // CaptureAreaScale = TaskContext.Instance().SystemInfo.CaptureAreaScale; // 捕获区域缩放比例（被注释掉）
//
//         // 设置 OffsetX 属性
//         OffsetX = offsetX;
//         // 设置 OffsetY 属性
//         OffsetY = offsetY;
//         // 设置 AssetScale 属性
//         AssetScale = assetScale;
//     }
//
//     // 执行点击操作
//     public void Click(int x, int y)
//     {
//         // 计算并执行带缩放的点击操作
//         ClickExtension.Click(OffsetX + (int)(x * AssetScale), OffsetY + (int)(y * AssetScale));
//     }
//
//     /// <summary>
//     /// 输入的x,y 注意处理缩放
//     /// </summary>
//     /// <param name="x"></param>
//     /// <param name="y"></param>
//     public void ClickWithoutScale(int x, int y)
//     {
//         // 执行不考虑缩放的点击操作
//         ClickExtension.Click(OffsetX + x, OffsetY + y);
//     }
//
//     // 执行移动操作
//     public void Move(int x, int y)
//     {
//         // 计算并执行带缩放的移动操作
//         ClickExtension.Move(OffsetX + (int)(x * AssetScale), OffsetY + (int)(y * AssetScale));
//     }
// }
```