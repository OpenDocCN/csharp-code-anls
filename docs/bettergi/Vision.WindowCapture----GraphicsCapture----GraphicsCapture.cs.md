# `.\better-genshin-impact\Vision.WindowCapture\GraphicsCapture\GraphicsCapture.cs`

```cs
﻿using System.Drawing; // 引入处理图形和图像的类库
using Vision.WindowCapture.GraphicsCapture.Helpers; // 引入自定义的帮助类
using Windows.Foundation.Metadata; // 引入用于处理 Windows API 信息的类
using Windows.Graphics.Capture; // 引入用于图形捕捉的类
using Windows.Graphics.DirectX; // 引入 DirectX 相关的类
using Windows.Win32.Foundation; // 引入 Windows 基础 API 的类

namespace Vision.WindowCapture.GraphicsCapture; // 定义命名空间

public class GraphicsCapture : IWindowCapture // 定义一个实现 IWindowCapture 接口的类
{
    private HWND _hWnd; // 声明一个私有字段，用于存储窗口句柄

    private Direct3D11CaptureFramePool? _captureFramePool; // 声明一个私有字段，用于存储 Direct3D11 捕捉帧池对象
    private GraphicsCaptureItem? _captureItem; // 声明一个私有字段，用于存储图形捕捉项对象
    private GraphicsCaptureSession? _captureSession; // 声明一个私有字段，用于存储图形捕捉会话对象

    public bool IsCapturing { get; private set; } // 定义一个公开属性，用于指示是否正在捕捉

    public async Task StartAsync(HWND hWnd) // 定义一个异步方法，用于启动捕捉
    {
        _hWnd = hWnd; // 存储传入的窗口句柄

        /*
        // if use GraphicsCapturePicker, need to set this window handle
        // new WindowInteropHelper(this).Handle
        var picker = new GraphicsCapturePicker(); // 创建一个图形捕捉选择器对象
        picker.SetWindow(hWnd); // 设置捕捉窗口
        _captureItem = picker.PickSingleItemAsync().AsTask().Result; // 异步选择捕捉项并存储
        */

        var device = Direct3D11Helper.CreateDevice(); // 创建一个 Direct3D11 设备

        _captureItem = CaptureHelper.CreateItemForWindow(_hWnd) ?? throw new InvalidOperationException("Failed to create capture item."); // 创建捕捉项，如果失败则抛出异常

        var size = _captureItem.Size; // 获取捕捉项的大小

        if (size.Width < 480 || size.Height < 360) // 检查捕捉项的大小是否小于最小要求
        {
            throw new InvalidOperationException("窗口画面大小小于480x360，无法使用"); // 如果大小不符合要求，则抛出异常
        }

        _captureItem.Closed += OnCaptureItemClosed; // 注册捕捉项关闭事件的处理程序

        _captureFramePool = Direct3D11CaptureFramePool.Create(device, DirectXPixelFormat.B8G8R8A8UIntNormalized, 2, size); // 创建 Direct3D11 捕捉帧池
        _captureSession = _captureFramePool.CreateCaptureSession(_captureItem); // 创建捕捉会话
        if (ApiInformation.IsWriteablePropertyPresent("Windows.Graphics.Capture.GraphicsCaptureSession", "IsBorderRequired")) // 检查是否支持设置捕捉会话的边框要求属性
        {
            await GraphicsCaptureAccess.RequestAccessAsync(GraphicsCaptureAccessKind.Borderless); // 请求无边框捕捉的访问权限
            _captureSession.IsBorderRequired = false; // 设置捕捉会话的边框要求为 false
            _captureSession.IsCursorCaptureEnabled = false; // 禁用光标捕捉
        }

        _captureSession.StartCapture(); // 启动捕捉会话
        IsCapturing = true; // 更新捕捉状态
    }

    /// <summary>
    /// How to handle window size changes?
    /// </summary>
    /// <returns></returns>
    public Bitmap? Capture() // 定义一个方法，用于捕捉当前帧并返回 Bitmap 对象
    {
        using var frame = _captureFramePool?.TryGetNextFrame(); // 尝试获取下一帧并使用
        return frame?.ToBitmap(); // 将帧转换为 Bitmap 对象并返回
    }

    public Task StopAsync() // 定义一个异步方法，用于停止捕捉
    {
        _captureSession?.Dispose(); // 释放捕捉会话资源
        _captureFramePool?.Dispose(); // 释放捕捉帧池资源
        _captureSession = default; // 清空捕捉会话对象
        _captureFramePool = default; // 清空捕捉帧池对象
        _captureItem = default; // 清空捕捉项对象

        _hWnd = HWND.Null; // 清空窗口句柄
        IsCapturing = false; // 更新捕捉状态
        return Task.CompletedTask; // 返回已完成的任务
    }

    private void OnCaptureItemClosed(GraphicsCaptureItem sender, object args) // 定义捕捉项关闭事件的处理程序
    {
        StopAsync(); // 调用停止捕捉的方法
    }
}
```