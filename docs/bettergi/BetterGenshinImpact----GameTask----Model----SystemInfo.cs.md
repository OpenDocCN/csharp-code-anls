# `.\better-genshin-impact\BetterGenshinImpact\GameTask\Model\SystemInfo.cs`

```cs
# 引用 BetterGenshinImpact 游戏任务模型中的 Area 类
﻿using BetterGenshinImpact.GameTask.Model.Area;
# 引用 BetterGenshinImpact 辅助工具类
using BetterGenshinImpact.Helpers;
# 引用 OpenCVSharp 库，用于计算机视觉处理
using OpenCvSharp;
# 引用系统基本功能
using System;
# 引用系统调试功能
using System.Diagnostics;
# 引用 Vanara.PInvoke 库，用于 P/Invoke 调用
using Vanara.PInvoke;
# 引用系统绘图大小类（别名为 Size）
using Size = System.Drawing.Size;

# 定义 BetterGenshinImpact.GameTask.Model 命名空间
namespace BetterGenshinImpact.GameTask.Model
{
    # 定义 SystemInfo 类
    public class SystemInfo
    {
        /// <summary>
        /// 显示器分辨率 无缩放
        /// </summary>
        // 显示器的原始分辨率，不进行任何缩放
        public Size DisplaySize { get; }

        /// <summary>
        /// 游戏窗口内分辨率
        /// </summary>
        // 游戏窗口中实际显示的分辨率
        public RECT GameScreenSize { get; }

        /// <summary>
        /// 以1080P为标准的素材缩放比例,不会大于1
        /// 与 ZoomOutMax1080PRatio 相等
        /// </summary>
        // 以1080P分辨率为基准的素材缩放比例，最大值为1，与 ZoomOutMax1080PRatio 相等
        public double AssetScale { get; } = 1;

        /// <summary>
        /// 游戏区域比1080P缩小的比例
        /// 最大值为1
        /// </summary>
        // 游戏区域相对于1080P分辨率的缩小比例，最大值为1
        public double ZoomOutMax1080PRatio { get; } = 1;

        /// <summary>
        /// 捕获游戏区域缩放至1080P的比例
        /// </summary>
        // 捕获游戏区域时，缩放至1080P分辨率的比例
        public double ScaleTo1080PRatio { get; }

        /// <summary>
        /// 捕获窗口区域 和实际游戏画面一致
        /// CaptureAreaRect = GameScreenSize or GameWindowRect
        /// </summary>
        // 捕获的窗口区域，应该与实际游戏画面区域一致
        // CaptureAreaRect 等于 GameScreenSize 或 GameWindowRect
        public RECT CaptureAreaRect { get; set; }

        /// <summary>
        /// 捕获窗口区域 大于1080P则为1920x1080
        /// </summary>
        // 捕获窗口区域，如果大于1080P则设置为1920x1080
        public Rect ScaleMax1080PCaptureRect { get; set; }

        // 游戏进程对象
        public Process GameProcess { get; }

        // 游戏进程名称
        public string GameProcessName { get; }

        // 游戏进程ID
        public int GameProcessId { get; }

        // 桌面区域
        public DesktopRegion DesktopRectArea { get; }

        public SystemInfo(IntPtr hWnd)
        {
            // 根据窗口句柄获取进程对象
            var p = SystemControl.GetProcessByHandle(hWnd);
            // 如果无法获取进程对象，则抛出异常
            GameProcess = p ?? throw new ArgumentException("通过句柄获取游戏进程失败");
            // 获取进程名称
            GameProcessName = GameProcess.ProcessName;
            // 获取进程ID
            GameProcessId = GameProcess.Id;

            // 获取显示器的工作区大小
            DisplaySize = PrimaryScreen.WorkingArea;
            // 初始化桌面区域
            DesktopRectArea = new DesktopRegion();

            // 判断窗口是否被最小化
            if (User32.IsIconic(hWnd))
            {
                // 如果窗口最小化，则抛出异常
                throw new ArgumentException("游戏窗口不能最小化");
            }

            // 获取游戏屏幕的实际分辨率
            GameScreenSize = SystemControl.GetGameScreenRect(hWnd);
            // 如果游戏屏幕分辨率小于800x600，则抛出异常
            if (GameScreenSize.Width < 800 || GameScreenSize.Height < 600)
            {
                throw new ArgumentException("游戏窗口分辨率不得小于 800x600 ！");
            }

            // 根据游戏屏幕宽度设置缩放比例，保证素材缩放比例不会超过1
            if (GameScreenSize.Width < 1920)
            {
                ZoomOutMax1080PRatio = GameScreenSize.Width / 1920d;
                AssetScale = ZoomOutMax1080PRatio;
            }
            // 设置缩放至1080P的比例
            ScaleTo1080PRatio = GameScreenSize.Width / 1920d; // 1080P 为标准

            // 获取捕获区域
            CaptureAreaRect = SystemControl.GetCaptureRect(hWnd);
            // 如果捕获区域宽度大于1920，则根据比例调整为1920x1080
            if (CaptureAreaRect.Width > 1920)
            {
                var scale = CaptureAreaRect.Width / 1920d;
                ScaleMax1080PCaptureRect = new Rect(CaptureAreaRect.X, CaptureAreaRect.Y, 1920, (int)(CaptureAreaRect.Height / scale));
            }
            else
            {
                // 否则，保持捕获区域原样
                ScaleMax1080PCaptureRect = new Rect(CaptureAreaRect.X, CaptureAreaRect.Y, CaptureAreaRect.Width, CaptureAreaRect.Height);
            }
        }
    }
# 结束当前代码块或结构体
}
```