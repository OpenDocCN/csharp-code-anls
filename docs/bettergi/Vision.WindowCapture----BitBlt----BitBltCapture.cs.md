# `.\better-genshin-impact\Vision.WindowCapture\BitBlt\BitBltCapture.cs`

```cs
# 引入必要的命名空间
﻿using System.Diagnostics;
using System.Drawing;
using Windows.Win32.Foundation;
using Windows.Win32.Graphics.Gdi;
using static Windows.Win32.PInvoke;

namespace Vision.WindowCapture.BitBlt
{
    public class BitBltCapture : IWindowCapture
    {
        private HWND _hWnd;  # 存储窗口句柄

        public bool IsCapturing { get; private set; }  # 表示是否正在捕捉的属性

        public Task StartAsync(HWND hWnd)
        {
            _hWnd = hWnd;  # 初始化窗口句柄
            IsCapturing = true;  # 设置捕捉状态为真
            return Task.CompletedTask;  # 返回已完成的任务
        }

        public Bitmap? Capture()
        {
            if (_hWnd == IntPtr.Zero)  # 检查窗口句柄是否有效
            {
                return null;  # 无效时返回 null
            }

            try
            {
                GetWindowRect(_hWnd, out var windowRect);  # 获取窗口矩形区域
                var width = windowRect.Width;  # 获取窗口宽度
                var height = windowRect.Height;  # 获取窗口高度

                var hdcSrc = GetWindowDC(_hWnd);  # 获取窗口的设备上下文
                var hdcDest = CreateCompatibleDC(hdcSrc);  # 创建与源设备上下文兼容的目标设备上下文
                var hBitmap = CreateCompatibleBitmap(hdcSrc, width, height);  # 创建与源设备上下文兼容的位图
                var hOld = SelectObject(hdcDest, hBitmap);  # 选择位图到目标设备上下文
                Windows.Win32.PInvoke.BitBlt(hdcDest, 0, 0, width, height, hdcSrc, 0, 0, ROP_CODE.SRCCOPY);  # 进行位块传输，复制源到目标
                SelectObject(hdcDest, hOld);  # 恢复目标设备上下文中的旧对象
                DeleteDC(hdcDest);  # 删除目标设备上下文
                _ = ReleaseDC(_hWnd, hdcSrc);  # 释放源设备上下文

                var bitmap = Image.FromHbitmap(hBitmap);  # 从位图创建图像对象
                DeleteObject(hBitmap);  # 删除位图对象
                return bitmap;  # 返回捕捉到的图像
            }
            catch (Exception e)
            {
                Debug.WriteLine(e);  # 捕捉并输出异常
                return null;  # 异常时返回 null
            }
        }

        public Task StopAsync()
        {
            _hWnd = HWND.Null;  # 重置窗口句柄
            IsCapturing = false;  # 设置捕捉状态为假
            return Task.CompletedTask;  # 返回已完成的任务
        }
    }
}
```