# `.\better-genshin-impact\Fischless.GameCapture\Graphics\Helpers\Direct3D11Helper.cs`

```cs
# 引入系统运行时互操作性支持
﻿using System.Runtime.InteropServices;
# 引入 Direct3D11 图形库的 DirectX 头文件
using Windows.Graphics.DirectX.Direct3D11;
# 引入 WinRT 支持
using WinRT;

namespace Fischless.GameCapture.Graphics.Helpers;

public static class Direct3D11Helper
{
    # 定义 IInspectable 接口的 GUID
    internal static Guid IInspectable = new("AF86E2E0-B12D-4c6a-9C5A-D7AA65101E90");
    # 定义 ID3D11Resource 接口的 GUID
    internal static Guid ID3D11Resource = new("dc8e63f3-d12b-4952-b47b-5e45026a862d");
    # 定义 IDXGIAdapter3 接口的 GUID
    internal static Guid IDXGIAdapter3 = new("645967A4-1392-4310-A798-8053CE3E93FD");
    # 定义 ID3D11Device 接口的 GUID
    internal static Guid ID3D11Device = new("db6f6ddb-ac77-4e88-8253-819df9bbf140");
    # 定义 ID3D11Texture2D 接口的 GUID
    internal static Guid ID3D11Texture2D = new("6f15aaf2-d208-4e89-9ab4-489535d34f9c");

    # 定义一个 COM 接口 IDirect3DDxgiInterfaceAccess 的定义
    [ComImport]
    [Guid("A9B3D012-3DF2-4EE3-B8D1-8695F457D3C1")]
    [InterfaceType(ComInterfaceType.InterfaceIsIUnknown)]
    [ComVisible(true)]
    interface IDirect3DDxgiInterfaceAccess
    {
        # 获取指定接口的指针
        nint GetInterface([In] ref Guid iid);
    };

    # 导入 d3d11.dll 中的 CreateDirect3D11DeviceFromDXGIDevice 函数
    [DllImport(
        "d3d11.dll",
        EntryPoint = "CreateDirect3D11DeviceFromDXGIDevice",
        SetLastError = true,
        CharSet = CharSet.Unicode,
        ExactSpelling = true,
        CallingConvention = CallingConvention.StdCall
       )]
    static extern uint CreateDirect3D11DeviceFromDXGIDevice(nint dxgiDevice, out nint graphicsDevice);

    # 导入 d3d11.dll 中的 CreateDirect3D11SurfaceFromDXGISurface 函数
    [DllImport(
        "d3d11.dll",
        EntryPoint = "CreateDirect3D11SurfaceFromDXGISurface",
        SetLastError = true,
        CharSet = CharSet.Unicode,
        ExactSpelling = true,
        CallingConvention = CallingConvention.StdCall
       )]
    static extern uint CreateDirect3D11SurfaceFromDXGISurface(nint dxgiSurface, out nint graphicsSurface);

    # 创建一个 Direct3D 设备
    public static IDirect3DDevice CreateDevice()
    {
        return CreateDevice(false);
    }

    # 创建一个 Direct3D 设备，支持使用 WARP
    public static IDirect3DDevice CreateDevice(bool useWARP)
    {
        # 创建一个 SharpDX Direct3D11 设备
        var d3dDevice = new SharpDX.Direct3D11.Device(
            useWARP ? SharpDX.Direct3D.DriverType.Software : SharpDX.Direct3D.DriverType.Hardware,
            SharpDX.Direct3D11.DeviceCreationFlags.BgraSupport);
        # 从 SharpDX 设备创建一个 Direct3D 设备
        var device = CreateDirect3DDeviceFromSharpDXDevice(d3dDevice);
        return device;
    }

    # 从 SharpDX 设备创建一个 Direct3D 设备
    public static IDirect3DDevice CreateDirect3DDeviceFromSharpDXDevice(SharpDX.Direct3D11.Device d3dDevice)
    {
        IDirect3DDevice device = default!;

        # 获取 Direct3D 设备的 DXGI 接口
        using (var dxgiDevice = d3dDevice.QueryInterface<SharpDX.DXGI.Device3>())
        {
            # 使用 DXGI 设备创建 Direct3D 11 设备
            uint hr = CreateDirect3D11DeviceFromDXGIDevice(dxgiDevice.NativePointer, out nint pUnknown);

            if (hr == 0)
            {
                # 从 ABI 指针创建 Direct3D 设备接口
                device = MarshalInterface<IDirect3DDevice>.FromAbi(pUnknown);
                Marshal.Release(pUnknown);
            }
        }

        return device;
    }

    # 从 SharpDX 纹理创建 Direct3D 11 表面
    public static IDirect3DSurface CreateDirect3DSurfaceFromSharpDXTexture(SharpDX.Direct3D11.Texture2D texture)
    {
        IDirect3DSurface surface = default!;  // 初始化一个 IDirect3DSurface 对象，默认值为空

        // 获取 Direct3D 表面对应的 DXGI 接口
        using (var dxgiSurface = texture.QueryInterface<SharpDX.DXGI.Surface>())
        {
            // 使用 WinRT 互操作对象包装原生设备
            uint hr = CreateDirect3D11SurfaceFromDXGISurface(dxgiSurface.NativePointer, out nint pUnknown);

            if (hr == 0)
            {
                // 如果操作成功，则将未知指针转换为 IDirect3DSurface 对象
                surface = Marshal.GetObjectForIUnknown(pUnknown) as IDirect3DSurface;
                // 释放指针
                Marshal.Release(pUnknown);
            }
        }

        // 返回获取的 IDirect3DSurface 对象
        return surface;
    }

    public static SharpDX.Direct3D11.Device CreateSharpDXDevice(IDirect3DDevice device)
    {
        // 从 IDirect3DDevice 对象中获取 DXGI 接口访问对象
        var access = device.As<IDirect3DDxgiInterfaceAccess>();
        // 获取 ID3D11Device 接口指针
        var d3dPointer = access.GetInterface(ID3D11Device);
        // 创建 SharpDX 的 Direct3D 11 设备对象
        var d3dDevice = new SharpDX.Direct3D11.Device(d3dPointer);
        // 返回创建的 SharpDX 设备对象
        return d3dDevice;
    }

    public static SharpDX.Direct3D11.Texture2D CreateSharpDXTexture2D(IDirect3DSurface surface)
    {
        // 从 IDirect3DSurface 对象中获取 DXGI 接口访问对象
        var access = surface.As<IDirect3DDxgiInterfaceAccess>();
        // 获取 ID3D11Texture2D 接口指针
        var d3dPointer = access.GetInterface(ID3D11Texture2D);
        // 创建 SharpDX 的 Direct3D 11 纹理对象
        var d3dSurface = new SharpDX.Direct3D11.Texture2D(d3dPointer);
        // 返回创建的 SharpDX 纹理对象
        return d3dSurface;
    }
}  # 结束当前代码块或结构（可能是函数、类等的闭合符号）
```