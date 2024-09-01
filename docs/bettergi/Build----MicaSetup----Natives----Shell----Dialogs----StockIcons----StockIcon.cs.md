# `.\better-genshin-impact\Build\MicaSetup\Natives\Shell\Dialogs\StockIcons\StockIcon.cs`

```cs
﻿using MicaSetup.Natives;  // 引入本地方法的命名空间
using System;  // 引入系统基础类库
using System.Drawing;  // 引入绘图相关类库
using System.Runtime.InteropServices;  // 引入用于非托管代码调用的类库
using System.Windows;  // 引入 WPF 核心类库
using System.Windows.Interop;  // 引入 WPF 和 Windows 窗体互操作类库
using System.Windows.Media.Imaging;  // 引入图像处理类库

namespace MicaSetup.Shell.Dialogs;  // 定义命名空间

public class StockIcon : IDisposable  // 定义 StockIcon 类，实现 IDisposable 接口
{
    private StockIconIdentifier identifier = StockIconIdentifier.Application;  // 图标标识符，默认值为应用程序图标
    private StockIconSize currentSize = StockIconSize.Large;  // 图标大小，默认值为大型
    private bool linkOverlay;  // 是否为链接叠加图标的标志
    private bool selected;  // 是否选中图标的标志
    private bool invalidateIcon = true;  // 是否需要更新图标的标志，默认值为需要
    private nint hIcon = 0;  // 存储图标句柄

    public StockIcon(StockIconIdentifier id)  // 构造函数，接受图标标识符
    {
        identifier = id;  // 设置图标标识符
        invalidateIcon = true;  // 设置需要更新图标
    }

    public StockIcon(StockIconIdentifier id, StockIconSize size, bool isLinkOverlay, bool isSelected)  // 构造函数，接受图标标识符、大小、链接叠加标志和选中标志
    {
        identifier = id;  // 设置图标标识符
        linkOverlay = isLinkOverlay;  // 设置链接叠加标志
        selected = isSelected;  // 设置选中标志
        currentSize = size;  // 设置图标大小
        invalidateIcon = true;  // 设置需要更新图标
    }

    public bool Selected  // 选中属性
    {
        get => selected;  // 获取选中状态
        set
        {
            selected = value;  // 设置选中状态
            invalidateIcon = true;  // 设置需要更新图标
        }
    }

    public bool LinkOverlay  // 链接叠加属性
    {
        get => linkOverlay;  // 获取链接叠加状态
        set
        {
            linkOverlay = value;  // 设置链接叠加状态
            invalidateIcon = true;  // 设置需要更新图标
        }
    }

    public StockIconSize CurrentSize  // 图标大小属性
    {
        get => currentSize;  // 获取当前图标大小
        set
        {
            currentSize = value;  // 设置图标大小
            invalidateIcon = true;  // 设置需要更新图标
        }
    }

    public StockIconIdentifier Identifier  // 图标标识符属性
    {
        get => identifier;  // 获取图标标识符
        set
        {
            identifier = value;  // 设置图标标识符
            invalidateIcon = true;  // 设置需要更新图标
        }
    }

    public Bitmap Bitmap  // 获取图像的 Bitmap 对象
    {
        get
        {
            UpdateHIcon();  // 更新图标句柄

            return hIcon != 0 ? Bitmap.FromHicon(hIcon) : null!;  // 根据图标句柄创建 Bitmap 对象，若句柄无效则返回 null
        }
    }

    public BitmapSource BitmapSource  // 获取图像的 BitmapSource 对象
    {
        get
        {
            UpdateHIcon();  // 更新图标句柄

            return (hIcon != 0) ?
                Imaging.CreateBitmapSourceFromHIcon(hIcon, Int32Rect.Empty, null!) : null!;  // 根据图标句柄创建 BitmapSource 对象，若句柄无效则返回 null
        }
    }

    public Icon Icon  // 获取图标的 Icon 对象
    {
        get
        {
            UpdateHIcon();  // 更新图标句柄

            return hIcon != 0 ? Icon.FromHandle(hIcon) : null!;  // 根据图标句柄创建 Icon 对象，若句柄无效则返回 null
        }
    }

    private void UpdateHIcon()  // 更新图标句柄的方法
    {
        if (invalidateIcon)  // 如果需要更新图标
        {
            if (hIcon != 0)
                _ = User32.DestroyIcon(hIcon);  // 销毁现有图标句柄

            hIcon = GetHIcon();  // 获取新的图标句柄

            invalidateIcon = false;  // 设置不需要更新图标
        }
    }

    private nint GetHIcon()  // 获取图标句柄的方法
    {
        // 初始化标志变量，设置为 StockIconOptions.Handle
        var flags = StockIconsNativeMethods.StockIconOptions.Handle;

        // 根据当前大小设置标志位
        if (CurrentSize == StockIconSize.Small)
        {
            flags |= StockIconsNativeMethods.StockIconOptions.Small;
        }
        else if (CurrentSize == StockIconSize.ShellSize)
        {
            flags |= StockIconsNativeMethods.StockIconOptions.ShellSize;
        }
        else
        {
            flags |= StockIconsNativeMethods.StockIconOptions.Large;
        }

        // 如果选中，添加选中标志
        if (Selected)
        {
            flags |= StockIconsNativeMethods.StockIconOptions.Selected;
        }

        // 如果有链接覆盖，添加链接覆盖标志
        if (LinkOverlay)
        {
            flags |= StockIconsNativeMethods.StockIconOptions.LinkOverlay;
        }

        // 创建 StockIconInfo 结构体，并设置其结构大小
        var info = new StockIconsNativeMethods.StockIconInfo
        {
            StuctureSize = (uint)Marshal.SizeOf(typeof(StockIconsNativeMethods.StockIconInfo))
        };

        // 调用方法获取图标信息
        var hr = StockIconsNativeMethods.SHGetStockIconInfo(identifier, flags, ref info);

        // 检查方法调用是否成功
        if (hr != HResult.Ok)
        {
            // 如果参数无效，抛出异常
            if (hr == HResult.InvalidArguments)
            {
                throw new InvalidOperationException(
                    string.Format(System.Globalization.CultureInfo.InvariantCulture,
                    LocalizedMessages.StockIconInvalidGuid,
                    identifier));
            }

            // 返回 0 表示失败
            return 0;
        }

        // 返回图标句柄
        return info.Handle;
    }

    // 释放资源的虚拟方法
    protected virtual void Dispose(bool disposing)
    {
        // 如果是显式调用 Dispose，则可以释放托管资源
        if (disposing)
        {
        }

        // 如果图标句柄不为 0，销毁图标
        if (hIcon != 0)
            _ = User32.DestroyIcon(hIcon);
    }

    // 显式调用 Dispose 方法
    public void Dispose()
    {
        Dispose(true);
        GC.SuppressFinalize(this);
    }

    // 析构函数，用于释放非托管资源
    ~StockIcon()
    {
        Dispose(false);
    }
你能提供更多的代码内容，还是只是这个单独的 `}` 吗？
```