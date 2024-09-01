# `.\better-genshin-impact\BetterGenshinImpact\Helpers\PrimaryScreen.cs`

```cs
# 引用系统和图形库及 Vanara.PInvoke 库
﻿using System;
using System.Drawing;
using Vanara.PInvoke;
using static Vanara.PInvoke.Gdi32;

# 定义命名空间
namespace BetterGenshinImpact.Helpers
{
    # 定义 PrimaryScreen 类
    public class PrimaryScreen
    {
        /// <summary>
        /// 获取屏幕分辨率当前物理大小
        /// </summary>
        # 定义公共静态属性 WorkingArea
        public static Size WorkingArea
        {
            get
            {
                # 获取屏幕设备上下文的句柄
                var hdc = User32.GetDC(IntPtr.Zero);
                # 创建 Size 结构体实例，存储屏幕宽度和高度
                var size = new Size
                {
                    # 获取屏幕宽度
                    Width = Gdi32.GetDeviceCaps(hdc, DeviceCap.HORZRES),
                    # 获取屏幕高度
                    Height = Gdi32.GetDeviceCaps(hdc, DeviceCap.VERTRES)
                };
                # 释放屏幕设备上下文
                User32.ReleaseDC(IntPtr.Zero, hdc);
                # 返回屏幕分辨率的大小
                return size;
            }
        }
    }
}
```