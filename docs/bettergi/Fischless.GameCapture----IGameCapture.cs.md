# `.\better-genshin-impact\Fischless.GameCapture\IGameCapture.cs`

```cs
# 定义命名空间，包含与游戏捕捉相关的类和接口
﻿namespace Fischless.GameCapture;

# 定义一个公共接口 IGameCapture 实现 IDisposable 接口，表示游戏捕捉的功能
public interface IGameCapture : IDisposable
{
    # 属性，获取捕捉模式
    public CaptureModes Mode { get; }
    # 属性，获取当前是否正在捕捉
    public bool IsCapturing { get; }

    # 方法，开始捕捉，接受一个窗口句柄和可选设置参数
    public void Start(nint hWnd, Dictionary<string, object>? settings = null);

    # 方法，进行捕捉并返回一个 Bitmap 对象，表示捕获的图像
    public Bitmap? Capture();

    # 方法，停止捕捉
    public void Stop();
}
```