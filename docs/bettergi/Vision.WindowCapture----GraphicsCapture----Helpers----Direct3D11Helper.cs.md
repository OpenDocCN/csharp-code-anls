# `.\better-genshin-impact\Vision.WindowCapture\GraphicsCapture\Helpers\Direct3D11Helper.cs`

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

using System.Runtime.InteropServices; // Import runtime interop services for handling unmanaged code
using Windows.Graphics.DirectX.Direct3D11; // Import Direct3D11 graphics library
using Windows.Win32; // Import Windows API functions and constants
using Windows.Win32.Foundation; // Import Windows foundation types and functions
using Windows.Win32.Graphics.Direct3D; // Import Direct3D graphics functions
using Windows.Win32.Graphics.Direct3D11; // Import Direct3D11 specific functions
using Windows.Win32.Graphics.Dxgi; // Import DirectX Graphics Infrastructure (DXGI) functions
using Windows.Win32.System.WinRT.Direct3D11; // Import WinRT Direct3D11 functions
using WinRT; // Import WinRT types and functions
using static Windows.Win32.PInvoke; // Import static PInvoke methods

namespace Vision.WindowCapture.GraphicsCapture.Helpers; // Define namespace for the helper class

public static class Direct3D11Helper
{
    // Create a Direct3D device and return it
    public static IDirect3DDevice? CreateDevice()
    {
        return CreateDirect3DDevice(); // Call method to create a Direct3D device
    }

    // Create a Direct3D device, handle errors by falling back to software device if necessary
    public static unsafe IDirect3DDevice? CreateDirect3DDevice()
    {
        // Try creating a hardware Direct3D device
        HRESULT hr = D3D11CreateDevice(
            default, // Default adapter
            D3D_DRIVER_TYPE.D3D_DRIVER_TYPE_HARDWARE, // Hardware driver type
            default, // Default software rasterizer
            D3D11_CREATE_DEVICE_FLAG.D3D11_CREATE_DEVICE_BGRA_SUPPORT, // Device creation flag
            default, // Default feature levels
            0, // Number of feature levels
            D3D11_SDK_VERSION, // SDK version
            out ID3D11Device? d3d11Device, // Output parameter for the Direct3D11 device
            default, // Default feature level
            out _); // Output parameter for the Direct3D11 device context (not used)

        // If hardware device creation is unsupported, fallback to software device
        if (hr == HRESULT.DXGI_ERROR_UNSUPPORTED)
        {
            D3D11CreateDevice(
                default, // Default adapter
                D3D_DRIVER_TYPE.D3D_DRIVER_TYPE_SOFTWARE, // Software driver type
                default, // Default software rasterizer
                D3D11_CREATE_DEVICE_FLAG.D3D11_CREATE_DEVICE_BGRA_SUPPORT, // Device creation flag
                default, // Default feature levels
                0, // Number of feature levels
                D3D11_SDK_VERSION, // SDK version
                out d3d11Device, // Output parameter for the Direct3D11 device
                default, // Default feature level
                out _); // Output parameter for the Direct3D11 device context (not used)
        }

        // Create and return a Direct3D device from the Direct3D11 device
        return CreateDirect3DDeviceFromD3D11Device(d3d11Device);
    }

    // Convert a Direct3D11 device to a Direct3D device
    public static IDirect3DDevice? CreateDirect3DDeviceFromD3D11Device(ID3D11Device d3d11Device)
    {
        // 获取 Direct3D 设备的 DXGI 接口。
        // 使用 WinRT 互操作对象包装本地设备。
        if (CreateDirect3D11DeviceFromDXGIDevice(d3d11Device.As<IDXGIDevice>(), out var iInspectable).Succeeded)
        {
            // 从 iInspectable 对象获取 IUnknown 指针
            nint thisPtr = Marshal.GetIUnknownForObject(iInspectable);
            // 使用指针创建 IDirect3DDevice 对象并返回
            return MarshalInterface<IDirect3DDevice>.FromAbi(thisPtr);
        }

        // 如果操作失败，返回默认值
        return default;
    }

    public static IDirect3DSurface? CreateDirect3DSurfaceFromSharpDXTexture(ID3D11Texture2D texture)
    {
        // 获取 Direct3D 表面的 DXGI 接口。
        // 使用 WinRT 互操作对象包装本地表面。
        if (CreateDirect3D11SurfaceFromDXGISurface(texture.As<IDXGISurface>(), out var iInspectable).Succeeded)
        {
            // 从 iInspectable 对象获取 IUnknown 指针
            nint thisPtr = Marshal.GetIUnknownForObject(iInspectable);
            // 使用指针创建 IDirect3DSurface 对象并返回
            return MarshalInterface<IDirect3DSurface>.FromAbi(thisPtr);
        }

        // 如果操作失败，返回默认值
        return default;
    }

    public static ID3D11Device CreateD3D11Device(IDirect3DDevice device)
    {
        // 从 IDirect3DDevice 对象获取 ID3D11Device 接口
        device
            .As<IDirect3DDxgiInterfaceAccess>()
            .GetInterface(typeof(ID3D11Device).GUID, out var d3dDevice);
        // 获取 ID3D11Device 对象
        object obj = Marshal.GetObjectForIUnknown(d3dDevice);
        // 将对象转换为 ID3D11Device 并返回
        return obj.As<ID3D11Device>();
    }

    public static ID3D11Texture2D CreateD3D11Texture2D(IDirect3DSurface surface)
    {
        // 从 IDirect3DSurface 对象获取 ID3D11Texture2D 接口
        surface
            .As<IDirect3DDxgiInterfaceAccess>()
            .GetInterface(typeof(ID3D11Texture2D).GUID, out var d3dSurface);
        // 获取 ID3D11Texture2D 对象
        object obj = Marshal.GetObjectForIUnknown(d3dSurface);
        // 将对象转换为 ID3D11Texture2D 并返回
        return obj.As<ID3D11Texture2D>();
    }
# 结束了一个代码块（在某些编程语言中，比如C语言或C++，这表示一个函数、控制结构或类的结束）
}
```