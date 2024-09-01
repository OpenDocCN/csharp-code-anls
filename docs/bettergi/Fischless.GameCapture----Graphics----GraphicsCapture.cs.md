# `.\better-genshin-impact\Fischless.GameCapture\Graphics\GraphicsCapture.cs`

```cs
﻿using Fischless.GameCapture.Graphics.Helpers;  // 引入用于图形捕获的帮助类
using SharpDX.Direct3D11;  // 引入 Direct3D 11 的相关类
using System.Diagnostics;  // 引入调试相关类
using Vanara.PInvoke;  // 引入 Vanara 库中的 PInvoke 函数
using Windows.Foundation.Metadata;  // 引入 Windows 应用程序接口元数据
using Windows.Graphics.Capture;  // 引入 Windows 图形捕获相关类
using Windows.Graphics.DirectX;  // 引入 Windows DirectX 图形相关类

namespace Fischless.GameCapture.Graphics;  // 定义命名空间

public class GraphicsCapture : IGameCapture  // 定义 GraphicsCapture 类，实现 IGameCapture 接口
{
    private nint _hWnd;  // 存储窗口句柄

    private Direct3D11CaptureFramePool _captureFramePool = null!;  // 存储捕获帧池对象
    private GraphicsCaptureItem _captureItem = null!;  // 存储图形捕获项目对象
    private GraphicsCaptureSession _captureSession = null!;  // 存储图形捕获会话对象

    public CaptureModes Mode => CaptureModes.WindowsGraphicsCapture;  // 获取捕获模式，设置为 Windows 图形捕获
    public bool IsCapturing { get; private set; }  // 获取或设置是否正在捕获的状态

    private ResourceRegion? _region;  // 存储捕获区域

    private bool _useBitmapCache = false;  // 标记是否使用位图缓存
    private Bitmap? _currentBitmap;  // 存储当前位图

    public void Dispose() => Stop();  // 实现 IDisposable 接口，调用 Stop 方法停止捕获

    public void Start(nint hWnd, Dictionary<string, object>? settings = null)  // 启动捕获
    {
        _hWnd = hWnd;  // 设置窗口句柄

        _region = GetGameScreenRegion(hWnd);  // 获取游戏屏幕区域

        IsCapturing = true;  // 标记为正在捕获

        _captureItem = CaptureHelper.CreateItemForWindow(_hWnd);  // 创建图形捕获项目

        if (_captureItem == null)  // 如果创建捕获项目失败
        {
            throw new InvalidOperationException("Failed to create capture item.");  // 抛出异常
        }

        _captureItem.Closed += CaptureItemOnClosed;  // 注册捕获项目关闭事件处理程序

        var device = Direct3D11Helper.CreateDevice();  // 创建 Direct3D 11 设备

        _captureFramePool = Direct3D11CaptureFramePool.Create(device, DirectXPixelFormat.B8G8R8A8UIntNormalized, 2,  // 创建捕获帧池
            _captureItem.Size);

        if (settings != null && settings.TryGetValue("useBitmapCache", out object? value) && (bool)value)  // 如果提供了设置并且设置了使用位图缓存
        {
            _useBitmapCache = true;  // 启用位图缓存
            _captureFramePool.FrameArrived += OnFrameArrived;  // 注册帧到达事件处理程序
        }

        _captureSession = _captureFramePool.CreateCaptureSession(_captureItem);  // 创建捕获会话
        _captureSession.IsCursorCaptureEnabled = false;  // 禁用光标捕获
        if (ApiInformation.IsWriteablePropertyPresent("Windows.Graphics.Capture.GraphicsCaptureSession", "IsBorderRequired"))  // 检查是否可以设置边框要求属性
        {
            _captureSession.IsBorderRequired = false;  // 禁用边框要求
        }

        _captureSession.StartCapture();  // 启动捕获会话
        IsCapturing = true;  // 标记为正在捕获
    }

    /// <summary>
    /// 从 DwmGetWindowAttribute 的矩形 截取出 GetClientRect的矩形（游戏区域）
    /// </summary>
    /// <param name="hWnd"></param>
    /// <returns></returns>
    private ResourceRegion? GetGameScreenRegion(nint hWnd)  // 获取游戏屏幕区域
    # 获取窗口扩展样式
    var exStyle = User32.GetWindowLong(hWnd, User32.WindowLongFlags.GWL_EXSTYLE);
    # 检查窗口是否设置为最上层窗口
    if ((exStyle & (int)User32.WindowStylesEx.WS_EX_TOPMOST) != 0)
    {
        # 如果是最上层窗口，则返回 null
        return null;
    }

    # 创建一个 ResourceRegion 对象
    ResourceRegion region = new();
    # 获取窗口的扩展框架边界
    DwmApi.DwmGetWindowAttribute<RECT>(hWnd, DwmApi.DWMWINDOWATTRIBUTE.DWMWA_EXTENDED_FRAME_BOUNDS, out var windowRect);
    # 获取客户端区域
    User32.GetClientRect(_hWnd, out var clientRect);
    #POINT point = default; // 这个点和 DwmGetWindowAttribute 结果差1
    #User32.ClientToScreen(hWnd, ref point);

    # 设置 ResourceRegion 对象的左边界
    region.Left = 0;
    # 设置 ResourceRegion 对象的上边界
    region.Top = windowRect.Height - clientRect.Height;
    # 设置 ResourceRegion 对象的右边界
    region.Right = clientRect.Width;
    # 设置 ResourceRegion 对象的下边界
    region.Bottom = windowRect.Height;
    # 设置 ResourceRegion 对象的前边界
    region.Front = 0;
    # 设置 ResourceRegion 对象的后边界
    region.Back = 1;

    # 返回 ResourceRegion 对象
    return region;
}

# 当帧到达时触发的方法
private void OnFrameArrived(Direct3D11CaptureFramePool sender, object args)
{
    # 尝试获取下一帧
    using var frame = _captureFramePool?.TryGetNextFrame();

    # 如果成功获取到帧
    if (frame != null)
    {
        # 将帧转换为 Bitmap 对象
        var b = frame.ToBitmap(_region);
        # 如果转换成功
        if (b != null)
        {
            # 更新当前 Bitmap 对象
            _currentBitmap = b;
        }
    }
}

# 捕获当前帧并返回 Bitmap 对象
public Bitmap? Capture()
{
    # 如果窗口句柄无效
    if (_hWnd == IntPtr.Zero)
    {
        return null;
    }

    # 如果不使用 Bitmap 缓存
    if (!_useBitmapCache)
    {
        try
        {
            # 尝试获取下一帧
            using var frame = _captureFramePool?.TryGetNextFrame();

            # 如果获取不到帧
            if (frame == null)
            {
                return null;
            }

            # 将帧转换为 Bitmap 对象并返回
            return frame.ToBitmap(_region);
        }
        catch (Exception e)
        {
            # 捕获异常并输出调试信息
            Debug.WriteLine(e);
        }

        # 如果发生异常则返回 null
        return null;
    }
    else
    {
        # 如果使用 Bitmap 缓存且缓存为空
        if (_currentBitmap == null)
        {
            return null;
        }

        # 返回 Bitmap 对象的克隆
        return (Bitmap)_currentBitmap.Clone();
    }
}

# 停止捕获并释放资源
public void Stop()
{
    # 释放捕获会话和帧池资源
    _captureSession?.Dispose();
    _captureFramePool?.Dispose();
    # 将资源变量设为 null
    _captureSession = null!;
    _captureFramePool = null!;
    _captureItem = null!;

    # 重置窗口句柄
    _hWnd = IntPtr.Zero;
    # 设置捕获状态为 false
    IsCapturing = false;
}

# 当捕获项关闭时触发的方法
private void CaptureItemOnClosed(GraphicsCaptureItem sender, object args)
{
    # 调用 Stop 方法停止捕获
    Stop();
}
# 结束当前代码块或语句
}
```