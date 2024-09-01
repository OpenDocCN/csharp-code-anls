# `.\better-genshin-impact\Fischless.GameCapture\Graphics\Helpers\Texture2DExtensions.cs`

```cs
# 定义一个静态类，用于处理 Direct3D11 纹理到 Bitmap 的转换
public static class Texture2DExtensions
{
    # 扩展方法，将 Direct3D11CaptureFrame 转换为 Bitmap
    public static Bitmap? ToBitmap(this Direct3D11CaptureFrame frame, ResourceRegion? region = null)
    {
        # 创建 SharpDX Texture2D 对象
        var texture2dBitmap = Direct3D11Helper.CreateSharpDXTexture2D(frame.Surface);

        # 获取设备对象
        var d3dDevice = texture2dBitmap.Device;

        # 创建一个用于临时存储的纹理
        var staging = new Texture2D(d3dDevice, new Texture2DDescription
        {
            Width = region == null ? frame.ContentSize.Width : region.Value.Right - region.Value.Left,
            Height = region == null ? frame.ContentSize.Height : region.Value.Bottom - region.Value.Top,
            MipLevels = 1,
            ArraySize = 1,
            Format = texture2dBitmap.Description.Format,
            Usage = ResourceUsage.Staging,
            SampleDescription = new SampleDescription(1, 0),
            BindFlags = BindFlags.None,
            CpuAccessFlags = CpuAccessFlags.Read,
            OptionFlags = ResourceOptionFlags.None
        });

        # 调用 CreateBitmap 方法将纹理数据转换为 Bitmap
        return staging.CreateBitmap(d3dDevice, texture2dBitmap, region);
    }

    # 扩展方法，将纹理数据转换为 Bitmap
    public static Bitmap? CreateBitmap(this Texture2D staging, SharpDX.Direct3D11.Device d3dDevice, Texture2D surfaceTexture, ResourceRegion? region = null)
    {
        try
        {
            # 根据是否有区域信息，选择不同的拷贝方式
            if (region != null)
            {
                d3dDevice.ImmediateContext.CopySubresourceRegion(surfaceTexture, 0, region, staging, 0);
            }
            else
            {
                d3dDevice.ImmediateContext.CopyResource(surfaceTexture, staging);
            }

            # 映射 staging 纹理的子资源以读取数据
            var dataBox = d3dDevice.ImmediateContext.MapSubresource(staging, 0, 0, MapMode.Read,
                SharpDX.Direct3D11.MapFlags.None,
                out DataStream stream);

            # 创建 Bitmap 对象
            var bitmap = new Bitmap(staging.Description.Width, staging.Description.Height, dataBox.RowPitch,
                PixelFormat.Format32bppArgb, dataBox.DataPointer);

            return bitmap;
        }
        catch (Exception e)
        {
            # 捕获异常并记录错误信息
            Debug.WriteLine("Failed to copy texture to bitmap.");
            Debug.WriteLine(e.StackTrace);
            return null;
        }
        finally
        {
            # 释放 staging 纹理资源
            staging.Dispose();
        }
    }
}
```