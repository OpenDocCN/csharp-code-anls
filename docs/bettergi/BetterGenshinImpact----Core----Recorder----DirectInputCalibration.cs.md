# `.\better-genshin-impact\BetterGenshinImpact\Core\Recorder\DirectInputCalibration.cs`

```cs
﻿// 引用系统库和其他相关库
// using System;
// using System.Diagnostics;
// using System.Threading.Tasks;
// using BetterGenshinImpact.Core.Monitor;
// using BetterGenshinImpact.Core.Simulator;
// using BetterGenshinImpact.GameTask.AutoGeniusInvokation.Exception;
// using BetterGenshinImpact.GameTask;
// using BetterGenshinImpact.GameTask.Common.Element.Assets;
// using BetterGenshinImpact.GameTask.Common.Map;
// using BetterGenshinImpact.GameTask.Model.Area;
// using BetterGenshinImpact.GameTask.Model.Enum;
// using BetterGenshinImpact.View.Drawable;
// using BetterGenshinImpact.ViewModel.Pages;
// using Microsoft.Extensions.Logging;
// using OpenCvSharp;
// using Vanara.PInvoke;
// using static BetterGenshinImpact.GameTask.Common.TaskControl;
//
// namespace BetterGenshinImpact.Core.Recorder;
//
// /// <summary>
// /// DirectInput、鼠标移动距离、视角度数之间的校准
// /// </summary>
// [Obsolete] // 标记此类为过时，不推荐使用
// public class DirectInputCalibration
// {
//     // 视角偏移移动单位
//     private const int CharMovingUnit = 500;
//
//     // 异步方法，启动视角校准
//     public async void Start()
//     {
//         var hasLock = false; // 是否获取到锁
//         try
//         {
//             hasLock = await TaskSemaphore.WaitAsync(0); // 尝试获取异步锁
//             if (!hasLock) // 如果未获取到锁
//             {
//                 Logger.LogError("启动视角校准功能失败：当前存在正在运行中的独立任务，请不要重复执行任务！"); // 记录错误日志
//                 return; // 退出方法
//             }
//
//             Init(); // 初始化
//
//             // 异步任务，获取偏移角度
//             await Task.Run(() =>
//             {
//                 GetOffsetAngle(); // 获取视角偏移角度
//             });
//         }
//         catch (Exception e) // 捕获异常
//         {
//             Logger.LogError(e.Message); // 记录错误消息
//             Logger.LogDebug(e.StackTrace); // 记录堆栈跟踪
//         }
//         finally
//         {
//             VisionContext.Instance().DrawContent.ClearAll(); // 清除绘制内容
//             TaskTriggerDispatcher.Instance().SetCacheCaptureMode(DispatcherCaptureModeEnum.NormalTrigger); // 设置缓存触发模式
//             TaskSettingsPageViewModel.SetSwitchAutoFightButtonText(false); // 关闭自动战斗按钮
//             Logger.LogInformation("→ {Text}", "视角校准结束"); // 记录信息日志
//
//             if (hasLock) // 如果获取到了锁
//             {
//                 TaskSemaphore.Release(); // 释放锁
//             }
//         }
//     }
//
//     // 初始化方法
//     private void Init()
//     {
//         SystemControl.ActivateWindow(); // 激活窗口
//         Logger.LogInformation("→ {Text}", "视角校准，启动！"); // 记录信息日志
//         TaskTriggerDispatcher.Instance().SetCacheCaptureMode(DispatcherCaptureModeEnum.OnlyCacheCapture); // 设置只缓存捕获模式
//         Sleep(TaskContext.Instance().Config.TriggerInterval * 5); // 等待缓存图像
//     }
//
//     // 获取视角偏移角度
//     public int GetOffsetAngle()
//     {
//         var directInputMonitor = new DirectInputMonitor(); // 创建 DirectInput 监控器
//         var ms1 = directInputMonitor.GetMouseState(); // 获取当前鼠标状态
//         Logger.LogInformation("当前鼠标状态：{X} {Y}", ms1.X, ms1.Y); // 记录鼠标状态信息
//         var angle1 = GetCharacterOrientationAngle(); // 获取角色的方向角度
//         Simulation.SendInput.Mouse.MoveMouseBy(CharMovingUnit, 0); // 鼠标向右移动指定单位
//         Sleep(500); // 等待 500 毫秒
//
//         Simulation.SendInput.Keyboard.KeyDown(User32.VK.VK_W).Sleep(100).KeyUp(User32.VK.VK_W); // 模拟按下和释放 W 键
//         Sleep(1000); // 等待 1000 毫秒
//
//         var ms2 = directInputMonitor.GetMouseState(); // 获取移动后的鼠标状态
# 当前鼠标状态的日志信息，记录 X 和 Y 坐标
//         Logger.LogInformation("当前鼠标状态：{X} {Y}", ms2.X, ms2.Y);

# 获取当前角色的朝向角度
//         var angle2 = GetCharacterOrientationAngle();

# 计算角色朝向角度的偏移量
//         var angleOffset = angle2 - angle1;

# 计算鼠标 X 坐标的位移量
//         var directInputXOffset = ms2.X - ms1.X;

# 记录横向移动偏移量的日志信息，包括鼠标平移、角度转动和 DirectInput 移动的数值
//         Logger.LogInformation("横向移动偏移量校准：鼠标平移{CharMovingUnit}单位，角度转动{AngleOffset}，DirectInput移动{DirectInputXOffset}",
//             CharMovingUnit, angleOffset, directInputXOffset);

# 计算每度角度所需的 MouseMoveBy 距离
//         var angle2MouseMoveByX = CharMovingUnit * 1d / angleOffset;

# 计算每度角度所需的 DirectInput 移动单位
//         var angle2DirectInputX = directInputXOffset * 1d / angleOffset;

# 记录校准结果的日志信息，包括每度角度所需的 MouseMoveBy 距离和 DirectInput 移动单位
//         Logger.LogInformation("校准结果：视角每移动1度，需要MouseMoveBy的距离{Angle2MouseMoveByX}，需要DirectInput移动的单位{Angle2DirectInputX}",
//                        angle2MouseMoveByX, angle2DirectInputX);

# 返回角度偏移量
//         return angleOffset;
//     }

//     public Mat? GetMiniMapMat(ImageRegion ra)
//     {
//         # 查找 Paimon 菜单的元素
//         var paimon = ra.Find(ElementAssets.Instance.PaimonMenuRo);
//         # 如果找到 Paimon 菜单，则根据其位置创建一个新的矩阵区域
//         if (paimon.IsExist())
//         {
//             return new Mat(ra.SrcMat, new Rect(paimon.X + 24, paimon.Y - 15, 210, 210));
//         }

//         # 如果未找到 Paimon 菜单，则返回 null
//         return null;
//     }

//     public int GetCharacterOrientationAngle()
//     {
//         # 从调度器中获取矩形区域
//         var ra = GetRectAreaFromDispatcher();
//         # 获取小地图矩阵
//         var miniMapMat = GetMiniMapMat(ra);
//         # 如果小地图矩阵为空，抛出异常
//         if (miniMapMat == null)
//         {
//             throw new InvalidOperationException("当前不在主界面");
//         }

//         # 计算角色的朝向角度
//         var angle = CharacterOrientation.Compute(miniMapMat);
//         # 记录当前角度的日志信息
//         Logger.LogInformation("当前角度：{Angle}", angle);
//         # 画出相机方向（注释掉的代码）
//         // CameraOrientation.DrawDirection(ra, angle);
//         # 返回角度
//         return angle;
//     }
// }
```