# `.\better-genshin-impact\Vision.WindowCapture\GraphicsCapture\Helpers\CaptureHelper.cs`

```cs
﻿//  ---------------------------------------------------------------------------------
//  Copyright (c) Microsoft Corporation.  All rights reserved.
// 
//  The MIT License (MIT)
// 
//  Permission is hereby granted, free of charge, to any person obtaining a copy
//  of this software and associated documentation files (the "Software"), to deal
//  in the Software without restriction, including without limitation the rights
//  to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
//  copies of the Software, and to permit persons to whom the Software is
//  furnished to do so, subject to the following conditions:
// 
//  The above copyright notice and this permission notice shall be included in
//  all copies or substantial portions of the Software.
// 
//  THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
//  IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
//  FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
//  AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
//  LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
//  OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN
//  THE SOFTWARE.
//  ---------------------------------------------------------------------------------

using System.Diagnostics.CodeAnalysis;  // 引入用于控制代码分析的特性
using Windows.Graphics.Capture;  // 引入 Windows.Graphics.Capture 命名空间，用于图形捕捉
using Windows.UI;  // 引入 Windows.UI 命名空间，包含与用户界面相关的类型
using Windows.Win32.Foundation;  // 引入 Windows.Win32.Foundation 命名空间，提供 Win32 API 相关的基础类型
using WinRT.Interop;  // 引入 WinRT.Interop 命名空间，用于 WinRT 互操作

namespace Vision.WindowCapture.GraphicsCapture.Helpers;  // 定义一个命名空间，用于组织代码

public static class CaptureHelper  // 定义一个静态类 CaptureHelper，用于图形捕捉的帮助方法
{
    public static void SetWindow(this GraphicsCapturePicker picker, IntPtr hwnd)  // 扩展方法，将窗口句柄与 GraphicsCapturePicker 绑定
    {
        InitializeWithWindow.Initialize(picker, hwnd);  // 调用 InitializeWithWindow.Initialize 方法，初始化捕捉器与窗口
    }

    [return: MaybeNull]  // 指示返回值可能为 null
    public static unsafe GraphicsCaptureItem CreateItemForWindow(HWND hwnd)  // 创建一个静态方法，根据窗口句柄创建 GraphicsCaptureItem
    {
        return GraphicsCaptureItem.TryCreateFromWindowId(*(WindowId*)&hwnd);  // 使用 TryCreateFromWindowId 方法创建 GraphicsCaptureItem，转换 HWND 为 WindowId
    }

    //public static GraphicsCaptureItem CreateItemForMonitor(IntPtr hmon)
    //{
    //    var factory = WindowsRuntimeMarshal.GetActivationFactory(typeof(GraphicsCaptureItem));  // 获取 GraphicsCaptureItem 的激活工厂
    //    var interop = (IGraphicsCaptureItemInterop)factory;  // 获取 IGraphicsCaptureItemInterop 接口
    //    var temp = typeof(GraphicsCaptureItem);  // 获取 GraphicsCaptureItem 类型信息
    //    var itemPointer = interop.CreateForMonitor(hmon, GraphicsCaptureItemGuid);  // 使用 CreateForMonitor 方法创建 GraphicsCaptureItem，传入监视器句柄和 GUID
    //    var item = Marshal.GetObjectForIUnknown(itemPointer) as GraphicsCaptureItem;  // 从 IUnknown 指针获取 GraphicsCaptureItem 对象
    //    Marshal.Release(itemPointer);  // 释放 IUnknown 指针

    //    return item;  // 返回创建的 GraphicsCaptureItem 对象
    //}
}
```