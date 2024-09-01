# `.\better-genshin-impact\Build\MicaSetup\Natives\Shell\Dialogs\Common\ShellThumbnail.cs`

```cs
﻿using MicaSetup.Natives;  // 导入 MicaSetup.Natives 命名空间
using System;  // 导入 System 命名空间
using System.Drawing;  // 导入 System.Drawing 命名空间
using System.Runtime.InteropServices;  // 导入 System.Runtime.InteropServices 命名空间
using System.Windows.Interop;  // 导入 System.Windows.Interop 命名空间
using System.Windows.Media.Imaging;  // 导入 System.Windows.Media.Imaging 命名空间

namespace MicaSetup.Shell.Dialogs;  // 定义 MicaSetup.Shell.Dialogs 命名空间

public class ShellThumbnail  // 定义 ShellThumbnail 类
{
    private readonly IShellItem shellItemNative;  // 定义只读字段 shellItemNative，用于存储原生 Shell 项

    private System.Windows.Size currentSize = new System.Windows.Size(256, 256);  // 定义字段 currentSize，默认设置为 256x256

    private ShellThumbnailFormatOption formatOption = ShellThumbnailFormatOption.Default;  // 定义字段 formatOption，默认格式选项为 Default

    internal ShellThumbnail(ShellObject shellObject)  // 定义内部构造函数，接收 ShellObject 实例
    {
        if (shellObject == null! || shellObject.NativeShellItem == null)  // 检查 shellObject 或其 NativeShellItem 是否为 null
        {
            throw new ArgumentNullException("shellObject");  // 如果为 null，则抛出 ArgumentNullException
        }

        shellItemNative = shellObject.NativeShellItem;  // 将 shellObject 的 NativeShellItem 赋值给 shellItemNative
    }

    public bool AllowBiggerSize { get; set; }  // 定义允许更大尺寸的属性

    public BitmapSource BitmapSource => GetBitmapSource(CurrentSize);  // 通过当前尺寸获取 BitmapSource

    public System.Windows.Size CurrentSize  // 定义 CurrentSize 属性
    {
        get => currentSize;  // 获取 currentSize 的值
        set  // 设置 currentSize 的值
        {
            if (value.Height == 0 || value.Width == 0)  // 如果高度或宽度为 0
            {
                throw new System.ArgumentOutOfRangeException("value", LocalizedMessages.ShellThumbnailSizeCannotBe0);  // 抛出 ArgumentOutOfRangeException
            }

            var size = (FormatOption == ShellThumbnailFormatOption.IconOnly) ?  // 根据 FormatOption 决定最大尺寸
                DefaultIconSize.Maximum : DefaultThumbnailSize.Maximum;

            if (value.Height > size.Height || value.Width > size.Width)  // 如果当前尺寸大于最大尺寸
            {
                throw new System.ArgumentOutOfRangeException("value",
                    string.Format(System.Globalization.CultureInfo.InvariantCulture,
                    LocalizedMessages.ShellThumbnailCurrentSizeRange, size.ToString()));  // 抛出 ArgumentOutOfRangeException
            }

            currentSize = value;  // 将新尺寸赋值给 currentSize
        }
    }

    public Bitmap ExtraLargeBitmap => GetBitmap(DefaultIconSize.ExtraLarge, DefaultThumbnailSize.ExtraLarge);  // 获取超大尺寸的 Bitmap

    public BitmapSource ExtraLargeBitmapSource => GetBitmapSource(DefaultIconSize.ExtraLarge, DefaultThumbnailSize.ExtraLarge);  // 获取超大尺寸的 BitmapSource

    public Icon ExtraLargeIcon => Icon.FromHandle(ExtraLargeBitmap.GetHicon());  // 从超大 Bitmap 创建 Icon

    public ShellThumbnailFormatOption FormatOption  // 定义 FormatOption 属性
    {
        get => formatOption;  // 获取 formatOption 的值
        set  // 设置 formatOption 的值
        {
            formatOption = value;  // 赋值 formatOption

            if (FormatOption == ShellThumbnailFormatOption.IconOnly  // 如果 FormatOption 为 IconOnly
                && (CurrentSize.Height > DefaultIconSize.Maximum.Height || CurrentSize.Width > DefaultIconSize.Maximum.Width))  // 当前尺寸大于最大图标尺寸
            {
                CurrentSize = DefaultIconSize.Maximum;  // 将 CurrentSize 设置为最大图标尺寸
            }
        }
    }

    public Icon Icon => Icon.FromHandle(Bitmap.GetHicon());  // 从 Bitmap 创建 Icon

    public Bitmap LargeBitmap => GetBitmap(DefaultIconSize.Large, DefaultThumbnailSize.Large);  // 获取大尺寸的 Bitmap

    public BitmapSource LargeBitmapSource => GetBitmapSource(DefaultIconSize.Large, DefaultThumbnailSize.Large);  // 获取大尺寸的 BitmapSource

    public Icon LargeIcon => Icon.FromHandle(LargeBitmap.GetHicon());  // 从大 Bitmap 创建 Icon

    public Bitmap MediumBitmap => GetBitmap(DefaultIconSize.Medium, DefaultThumbnailSize.Medium);  // 获取中等尺寸的 Bitmap
    # 获取中等大小的 BitmapSource 对象，调用指定的图像大小和缩略图大小
    public BitmapSource MediumBitmapSource => GetBitmapSource(DefaultIconSize.Medium, DefaultThumbnailSize.Medium);

    # 从中等大小的 Bitmap 对象获取 Icon 对象
    public Icon MediumIcon => Icon.FromHandle(MediumBitmap.GetHicon());

    # 获取或设置缩略图检索选项
    public ShellThumbnailRetrievalOption RetrievalOption { get; set; }

    # 获取小尺寸的 Bitmap 对象，调用指定的图像大小和缩略图大小
    public Bitmap SmallBitmap => GetBitmap(DefaultIconSize.Small, DefaultThumbnailSize.Small);

    # 获取小尺寸的 BitmapSource 对象，调用指定的图像大小和缩略图大小
    public BitmapSource SmallBitmapSource => GetBitmapSource(DefaultIconSize.Small, DefaultThumbnailSize.Small);

    # 从小尺寸的 Bitmap 对象获取 Icon 对象
    public Icon SmallIcon => Icon.FromHandle(SmallBitmap.GetHicon());

    # 获取当前尺寸的 Bitmap 对象
    public Bitmap Bitmap => GetBitmap(CurrentSize);

    # 计算标志位，根据允许的大小、检索选项和格式选项设置标志
    private SIIGBF CalculateFlags()
    {
        # 初始化标志位为 0
        SIIGBF flags = 0x0000;

        # 如果允许更大的尺寸，设置 BiggerSizeOk 标志
        if (AllowBiggerSize)
        {
            flags |= SIIGBF.BiggerSizeOk;
        }

        # 根据检索选项设置 InCacheOnly 或 MemoryOnly 标志
        if (RetrievalOption == ShellThumbnailRetrievalOption.CacheOnly)
        {
            flags |= SIIGBF.InCacheOnly;
        }
        else if (RetrievalOption == ShellThumbnailRetrievalOption.MemoryOnly)
        {
            flags |= SIIGBF.MemoryOnly;
        }

        # 根据格式选项设置 IconOnly 或 ThumbnailOnly 标志
        if (FormatOption == ShellThumbnailFormatOption.IconOnly)
        {
            flags |= SIIGBF.IconOnly;
        }
        else if (FormatOption == ShellThumbnailFormatOption.ThumbnailOnly)
        {
            flags |= SIIGBF.ThumbnailOnly;
        }

        # 返回计算后的标志位
        return flags;
    }

    # 根据格式选项选择图标大小或缩略图大小，并获取 Bitmap 对象
    private Bitmap GetBitmap(System.Windows.Size iconOnlySize, System.Windows.Size thumbnailSize) => GetBitmap(FormatOption == ShellThumbnailFormatOption.IconOnly ? iconOnlySize : thumbnailSize);

    # 获取指定大小的 Bitmap 对象
    private Bitmap GetBitmap(System.Windows.Size size)
    {
        # 根据指定大小获取 HBITMAP 句柄
        var hBitmap = GetHBitmap(size);

        # 从 HBITMAP 句柄创建 Bitmap 对象
        var returnValue = Bitmap.FromHbitmap(hBitmap);

        # 删除 HBITMAP 句柄以释放资源
        Gdi32.DeleteObject(hBitmap);

        # 返回创建的 Bitmap 对象
        return returnValue;
    }

    # 根据格式选项选择图标大小或缩略图大小，并获取 BitmapSource 对象
    private BitmapSource GetBitmapSource(System.Windows.Size iconOnlySize, System.Windows.Size thumbnailSize) => GetBitmapSource(FormatOption == ShellThumbnailFormatOption.IconOnly ? iconOnlySize : thumbnailSize);

    # 获取指定大小的 BitmapSource 对象
    private BitmapSource GetBitmapSource(System.Windows.Size size)
    {
        # 根据指定大小获取 HBITMAP 句柄
        var hBitmap = GetHBitmap(size);

        # 从 HBITMAP 句柄创建 BitmapSource 对象
        var returnValue = Imaging.CreateBitmapSourceFromHBitmap(
            hBitmap,
            IntPtr.Zero,
            System.Windows.Int32Rect.Empty,
            BitmapSizeOptions.FromEmptyOptions());

        # 删除 HBITMAP 句柄以释放资源
        Gdi32.DeleteObject(hBitmap);

        # 返回创建的 BitmapSource 对象
        return returnValue;
    }

    # 获取指定大小的 HBITMAP 句柄
    private nint GetHBitmap(System.Windows.Size size)
    # 创建一个 SIZE 结构体并初始化其宽度和高度
    var nativeSIZE = new SIZE()
    {
        Width = Convert.ToInt32(size.Width),  # 将 size.Width 转换为整数并赋值给 Width
        Height = Convert.ToInt32(size.Height) # 将 size.Height 转换为整数并赋值给 Height
    };

    # 调用 IShellItemImageFactory 的 GetImage 方法，获取图像的句柄
    var hr = ((IShellItemImageFactory)shellItemNative).GetImage(nativeSIZE, CalculateFlags(), out nint hbitmap);

    # 检查方法调用是否成功，返回图像句柄
    if (hr == HResult.Ok) { return hbitmap; }
    # 如果失败且错误码为 0x8004B200，并且格式选项为 ThumbnailOnly，则抛出 InvalidOperationException
    else if ((uint)hr == 0x8004B200 && FormatOption == ShellThumbnailFormatOption.ThumbnailOnly)
    {
        throw new InvalidOperationException(LocalizedMessages.ShellThumbnailDoesNotHaveThumbnail, Marshal.GetExceptionForHR((int)hr));
    }
    # 如果错误码为 0x80040154，则抛出 NotSupportedException
    else if ((uint)hr == 0x80040154)
    {
        throw new NotSupportedException(LocalizedMessages.ShellThumbnailNoHandler, Marshal.GetExceptionForHR((int)hr));
    }

    # 如果发生其他错误，则抛出 ShellException
    throw new ShellException(hr);
这个代码片段看起来是不完整的，能否提供更多的上下文或相关代码？
```