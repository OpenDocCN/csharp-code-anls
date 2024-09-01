# `.\better-genshin-impact\Fischless.GameCapture\BitBlt\BitBltCapture.cs`

```cs
# 使用 System.Diagnostics 和 Vanara.PInvoke 库进行屏幕捕捉
using System.Diagnostics;
using Vanara.PInvoke;

namespace Fischless.GameCapture.BitBlt;

public class BitBltCapture : IGameCapture
{
    # 存储窗口句柄
    private nint _hWnd;

    # 捕捉模式属性，返回 BitBlt 模式
    public CaptureModes Mode => CaptureModes.BitBlt;

    # 标记当前是否正在捕捉
    public bool IsCapturing { get; private set; }

    # 释放资源并停止捕捉
    public void Dispose() => Stop();

    # 启动捕捉，设置窗口句柄
    public void Start(nint hWnd, Dictionary<string, object>? settings = null)
    {
        _hWnd = hWnd;
        IsCapturing = true;
    }

    # 捕捉当前窗口的位图
    public Bitmap? Capture()
    {
        # 如果窗口句柄为零，则返回 null
        if (_hWnd == IntPtr.Zero)
        {
            return null;
        }

        try
        {
            # 获取窗口的客户区矩形
            User32.GetClientRect(_hWnd, out var windowRect);
            int x = default, y = default;
            # 计算窗口宽度和高度
            var width = windowRect.right - windowRect.left;
            var height = windowRect.bottom - windowRect.top;

            # 创建新的位图
            Bitmap bitmap = new(width, height);
            # 从位图创建绘图对象
            using System.Drawing.Graphics g = System.Drawing.Graphics.FromImage(bitmap);
            # 获取目标设备上下文
            var hdcDest = g.GetHdc();
            # 获取源设备上下文（窗口或桌面）
            var hdcSrc = User32.GetDC(_hWnd == IntPtr.Zero ? User32.GetDesktopWindow() : _hWnd);
            # 从源设备上下文拷贝图像到目标设备上下文
            Gdi32.StretchBlt(hdcDest, 0, 0, width, height, hdcSrc, x, y, width, height, Gdi32.RasterOperationMode.SRCCOPY);
            # 释放目标设备上下文
            g.ReleaseHdc();
            # 删除设备上下文
            Gdi32.DeleteDC(hdcDest);
            Gdi32.DeleteDC(hdcSrc);
            # 返回捕捉的位图
            return bitmap;
        }
        catch (Exception e)
        {
            # 捕捉异常并写入调试输出
            Debug.WriteLine(e);
        }

        # 捕捉失败时返回 null
        return null!;
    }

    # 停止捕捉并重置状态
    public void Stop()
    {
        _hWnd = IntPtr.Zero;
        IsCapturing = false;
    }
}
```