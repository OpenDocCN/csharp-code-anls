# `.\better-genshin-impact\Build\MicaSetup\Helper\Setup\ArchiveFileHelper.cs`

```cs
# 引入所需的库
﻿using MicaSetup.Controls.Animations;
using SharpCompress.Archives.GZip;
using SharpCompress.Archives.Rar;
using SharpCompress.Archives.SevenZip;
using SharpCompress.Archives.Zip;
using SharpCompress.Common;
using SharpCompress.Readers;
using System;
using System.IO;

namespace MicaSetup.Helper;

# 定义一个静态类 ArchiveFileHelper 用于处理归档文件
public static class ArchiveFileHelper
{
    # 计算指定文件路径的归档文件的总解压缩大小
    public static long TotalUncompressSize(string filePath, ReaderOptions? readerOptions = null!)
    {
        # 打开归档文件并自动释放资源，使用动态类型以处理不同归档格式
        using dynamic archive = filePath.OpenArchive(readerOptions);
        # 返回归档文件的总解压缩大小
        return archive.TotalUncompressSize;
    }

    # 计算从流中读取的归档文件的总解压缩大小
    public static long TotalUncompressSize(Stream stream, ReaderOptions? readerOptions = null!)
    {
        # 打开流中的归档文件并自动释放资源，使用动态类型以处理不同归档格式
        using dynamic archive = stream.OpenArchive(readerOptions);
        # 返回归档文件的总解压缩大小
        return archive.TotalUncompressSize;
    }

    # 从指定文件路径中提取所有文件到目标目录
    public static void ExtractAll(string destinationDirectory, string filePath, ReaderOptions? readerOptions = null!, ExtractionOptions? options = null)
    {
        # 打开归档文件并自动释放资源，使用动态类型以处理不同归档格式
        using dynamic archive = filePath.OpenArchive(readerOptions);
        # 获取归档文件的读取器并提取所有条目
        using IReader reader = archive.ExtractAllEntries();
        # 将所有条目写入目标目录
        reader.WriteAllToDirectory(destinationDirectory, options);
    }

    # 从流中提取所有文件到目标目录，并提供进度回调
    public static void ExtractAll(string destinationDirectory, Stream stream, Action<double, string> progressCallback = null!, ReaderOptions? readerOptions = null!, ExtractionOptions? options = null)
    {
        # 打开流中的归档文件并自动释放资源，使用动态类型以处理不同归档格式
        using dynamic archive = stream.OpenArchive(readerOptions);
        # 获取归档文件的读取器并提取所有条目
        using IReader reader = archive.ExtractAllEntries();
        # 创建一个进度累积器对象，使用双重缓动动画
        using ProgressAccumulator acc = new(anime: DoubleEasingAnimations.EaseInOutCirc);
        # 初始化当前总大小、当前进度和当前条目键
        long currentTotalSize = default;
        double currentProgress = default;
        string currentKey = null!;

        # 定义进度回调函数，更新进度和当前条目键
        void ProgressCallback(double progress)
        {
            if (currentProgress != progress || currentKey != reader.Entry.Key)
            {
                progressCallback?.Invoke(currentProgress = progress, currentKey = reader.Entry.Key);
            }
        }

        # 遍历所有条目并处理它们
        while (reader.MoveToNextEntry())
        {
            # 更新进度回调，计算当前进度
            ProgressCallback(Math.Min(currentTotalSize / (double)archive.TotalUncompressSize, 1d));

            # 如果条目大小大于 1MB，则启动进度动画
            if ((reader.Entry.Size / 1048576d) > 1d)
            {
                _ = acc.Reset(Math.Min(currentTotalSize / (double)archive.TotalUncompressSize, 1d),
                    Math.Min((currentTotalSize + reader.Entry.Size) / (double)archive.TotalUncompressSize, 1d),
                    reader.Entry.Size / 8912.896,
                    ProgressCallback
                ).Start();
            }
            # 将当前条目写入目标目录
            reader.WriteEntryToDirectory(destinationDirectory, options);

            # 停止进度动画，更新总大小
            acc.Stop();
            currentTotalSize += reader.Entry.Size;
            # 更新进度回调
            ProgressCallback(Math.Min(currentTotalSize / (double)archive.TotalUncompressSize, 1d));
        }
    }
}

# 定义一个文件枚举，表示支持的归档文件类型
file enum ArchiveFileType
{
    Unknown,
    SevenZip,
    Zip,
    Rar,
    GZip,
}

# 定义一个静态类，扩展 Stream 类型以获取归档文件类型
file static class ArchiveFileHelperExtension
{
    # 从流中获取归档文件类型
    public static ArchiveFileType GetArchiveType(this Stream stream)
    # 创建一个长度为 3 的字节数组，用于存储文件头部的字节数据
    byte[] header = new byte[3];
    # 从流中读取 3 个字节到 header 数组中
    stream.Read(header, 0, header.Length);

    # 判断文件头部字节是否匹配 7z 文件的标识
    if (header[0] == 0x37 && header[1] == 0x7A)
    {
        # 如果匹配，返回 SevenZip 类型
        return ArchiveFileType.SevenZip;
    }
    # 判断文件头部字节是否匹配 ZIP 文件的标识
    else if (header[0] == 0x50 && header[1] == 0x4B)
    {
        # 如果匹配，返回 Zip 类型
        return ArchiveFileType.Zip;
    }
    # 判断文件头部字节是否匹配 RAR 文件的标识
    else if (header[0] == 0x52 && header[1] == 0x61 && header[2] == 0x72)
    {
        # 如果匹配，返回 Rar 类型
        return ArchiveFileType.Rar;
    }
    # 判断文件头部字节是否匹配 GZip 文件的标识
    else if (header[0] == 0x1F && header[1] == 0x8B)
    {
        # 如果匹配，返回 GZip 类型
        return ArchiveFileType.GZip;
    }
    # 如果没有匹配的文件类型，返回 Unknown 类型
    return ArchiveFileType.Unknown;
    # 打开指定路径的文件并获取其流
    public static ArchiveFileType GetArchiveType(this string filePath)
    {
        # 打开文件流用于读取文件内容
        using Stream fileStream = filePath.OpenRead();
        # 获取文件的归档类型
        return GetArchiveType(fileStream);
    }

    # 打开指定路径的文件并返回文件流
    public static Stream OpenRead(this string filePath)
    {
        # 创建一个文件流，用于读取文件
        return new FileStream(filePath, FileMode.Open, FileAccess.Read);
    }

    # 根据文件路径打开对应的归档文件
    public static dynamic OpenArchive(this string filePath, ReaderOptions? readerOptions = null!)
    {
        # 获取文件的归档类型
        ArchiveFileType type = filePath.GetArchiveType();
        # 根据归档类型选择相应的归档打开方法
        dynamic? archive = type switch
        {
            ArchiveFileType.Zip => ZipArchive.Open(filePath, readerOptions),
            ArchiveFileType.GZip => GZipArchive.Open(filePath, readerOptions),
            ArchiveFileType.Rar => RarArchive.Open(filePath, readerOptions),
            ArchiveFileType.SevenZip or _ => SevenZipArchive.Open(filePath, readerOptions),
        };
        # 返回打开的归档对象
        return archive;
    }

    # 根据流打开对应的归档文件
    public static dynamic OpenArchive(this Stream stream, ReaderOptions? readerOptions = null!)
    {
        # 获取流的归档类型
        ArchiveFileType type = stream.GetArchiveType();
        # 根据归档类型选择相应的归档打开方法，并确保正确释放资源
        using dynamic? archive = type switch
        {
            ArchiveFileType.Zip => ZipArchive.Open(stream, readerOptions),
            ArchiveFileType.GZip => GZipArchive.Open(stream, readerOptions),
            ArchiveFileType.Rar => RarArchive.Open(stream, readerOptions),
            ArchiveFileType.SevenZip or _ => SevenZipArchive.Open(stream, readerOptions),
        };
        # 返回打开的归档对象
        return archive;
    }
# 结束一个代码块或语句块
}
```