# `.\better-genshin-impact\BetterGenshinImpact\Service\Notification\Converter\ImageToBase64Converter.cs`

```cs
# 引入必要的命名空间
﻿using System;
using System.Drawing;
using System.Drawing.Imaging;
using System.IO;
using System.Text.Json;
using System.Text.Json.Serialization;

# 定义一个用于将 Image 对象与 JSON 之间转换的类
namespace BetterGenshinImpact.Service.Notification.Converter;

# 创建一个从 Image 到 Base64 的 JSON 转换器
public class ImageToBase64Converter : JsonConverter<Image>
{
    # 读取 JSON 中的 Base64 字符串并将其转换为 Image 对象
    public override Image? Read(ref Utf8JsonReader reader, Type typeToConvert, JsonSerializerOptions options)
    {
        # 从 JSON 读取 Base64 字符串
        if (reader.GetString() is string base64)
        {
            # 从 Base64 字符串创建 MemoryStream，然后将其转换为 Image 对象
            return Image.FromStream(new MemoryStream(Convert.FromBase64String(base64)));
        }
        # 如果没有读取到 Base64 字符串，则返回默认值
        return default;
    }

    # 将 Image 对象转换为 Base64 字符串并写入 JSON
    public override void Write(Utf8JsonWriter writer, Image value, JsonSerializerOptions options)
    {
        # 使用 MemoryStream 保存 Image 对象
        using var ms = new MemoryStream();
        value.Save(ms, ImageFormat.Jpeg);
        # 将 Image 数据作为 Base64 字符串写入 JSON
        writer.WriteBase64StringValue(ms.ToArray());
    }
}
```