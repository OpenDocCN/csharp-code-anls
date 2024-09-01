# `.\better-genshin-impact\Fischless.GameCapture\Graphics\Helpers\CaptureHelper.cs`

```cs
# 引入用于与 COM 组件进行互操作的命名空间
﻿using System.Runtime.InteropServices;
# 引入用于图形捕获的 Windows 库
using Windows.Graphics.Capture;
# 引入用于 WinRT 组件的支持
using WinRT;

namespace Fischless.GameCapture.Graphics.Helpers;

# 定义一个静态帮助类用于图形捕获
public static class CaptureHelper
{
    # 定义一个静态只读的 GUID，用于标识图形捕获项
    static readonly Guid GraphicsCaptureItemGuid = new("79C3F95B-31F7-4EC2-A464-632EF5D30760");

    # 定义一个 COM 接口，用于初始化带有窗口句柄的对象
    [ComImport]
    [Guid("3E68D4BD-7135-4D10-8018-9FB6D9F33FA1")]
    [InterfaceType(ComInterfaceType.InterfaceIsIUnknown)]
    [ComVisible(true)]
    interface IInitializeWithWindow
    {
        # 初始化方法，接受窗口句柄
        void Initialize(
            nint hWnd);
    }

    # 定义一个 COM 接口，用于创建图形捕获项
    [ComImport]
    [Guid("3628E81B-3CAC-4C60-B7F4-23CE0E0C3356")]
    [InterfaceType(ComInterfaceType.InterfaceIsIUnknown)]
    [ComVisible(true)]
    interface IGraphicsCaptureItemInterop
    {
        # 根据窗口句柄创建图形捕获项
        nint CreateForWindow(
            [In] nint window,
            [In] ref Guid iid);

        # 根据监视器句柄创建图形捕获项
        nint CreateForMonitor(
            [In] nint monitor,
            [In] ref Guid iid);
    }

    # 扩展方法：将图形捕获选择器与窗口句柄关联
    public static void SetWindow(this GraphicsCapturePicker picker, nint hWnd)
    {
        # 将图形捕获选择器转换为 IInitializeWithWindow 接口
        var interop = picker.As<IInitializeWithWindow>();
        # 调用初始化方法，传入窗口句柄
        interop.Initialize(hWnd);
    }

    # 创建一个用于指定窗口的图形捕获项
    public static GraphicsCaptureItem CreateItemForWindow(nint hWnd)
    {
        # 获取 Windows.Graphics.Capture.GraphicsCaptureItem 的激活工厂
        var factory = WinrtModule.GetActivationFactory("Windows.Graphics.Capture.GraphicsCaptureItem");
        # 将激活工厂转换为 IGraphicsCaptureItemInterop 接口
        var interop = factory.AsInterface<IGraphicsCaptureItemInterop>();
        # 调用创建图形捕获项的方法，传入窗口句柄和 GUID
        var itemPointer = interop.CreateForWindow(hWnd, GraphicsCaptureItemGuid);
        # 将指针转换为 GraphicsCaptureItem 对象
        return GraphicsCaptureItem.FromAbi(itemPointer);
    }
}
```