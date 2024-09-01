# `.\better-genshin-impact\Vision.WindowCapture\IWindowCapture.cs`

```cs
# 引入系统绘图相关的命名空间
﻿using System.Drawing;
# 引入 Windows API 的基础设施相关的命名空间
using Windows.Win32.Foundation;

# 定义一个命名空间，用于组织和封装窗口捕捉相关的功能
namespace Vision.WindowCapture;

# 定义一个接口，声明窗口捕捉功能的方法和属性
public interface IWindowCapture
{
    # 属性，用于检查当前是否正在捕捉窗口
    bool IsCapturing { get; }

    # 异步方法，开始捕捉指定窗口，接受窗口句柄作为参数
    Task StartAsync(HWND hWnd);

    # 方法，用于捕捉当前窗口的图像，并返回一个可选的 Bitmap 对象
    Bitmap? Capture();

    # 异步方法，停止捕捉窗口
    Task StopAsync();
}
```