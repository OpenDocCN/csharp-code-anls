# `.\better-genshin-impact\BetterGenshinImpact\Service\ConfigService.cs`

```cs
﻿using BetterGenshinImpact.Core.Config; // 引入配置相关的核心模块
using BetterGenshinImpact.Service.Interface; // 引入服务接口定义
using OpenCvSharp; // 引入 OpenCV 库
using System; // 引入系统基本功能
using System.IO; // 引入文件操作功能
using System.Text.Json; // 引入 JSON 序列化和反序列化功能
using System.Text.Json.Serialization; // 引入 JSON 序列化和反序列化的自定义转换器
using System.Threading; // 引入线程同步功能

namespace BetterGenshinImpact.Service; // 定义命名空间

public class ConfigService : IConfigService // 定义 ConfigService 类，继承 IConfigService 接口
{
    private readonly object _locker = new(); // 定义一个锁对象，用于确保线程安全（在这里可能意义不大）
    private readonly ReaderWriterLockSlim _rwLock = new(); // 定义读写锁对象，用于多线程读写同步

    public static readonly JsonSerializerOptions JsonOptions = new() // 静态只读 JSON 序列化选项配置
    {
        NumberHandling = JsonNumberHandling.AllowNamedFloatingPointLiterals, // 允许命名浮点数字面量
        Encoder = System.Text.Encodings.Web.JavaScriptEncoder.UnsafeRelaxedJsonEscaping, // 使用放松的 JSON 转义编码器
        PropertyNamingPolicy = JsonNamingPolicy.CamelCase, // JSON 属性命名采用 camelCase
        Converters =
        {
            new OpenCvPointJsonConverter(), // 添加 OpenCvPoint 转换器
            new OpenCvRectJsonConverter(), // 添加 OpenCvRect 转换器
        },
        WriteIndented = true, // 格式化 JSON 输出（缩进）
        AllowTrailingCommas = true, // 允许 JSON 中的尾随逗号
        ReadCommentHandling = JsonCommentHandling.Skip, // 忽略 JSON 中的注释
    };

    /// <summary>
    /// 写入只有UI线程会调用
    /// 多线程只会读，放心用static，不会丢失数据
    /// </summary>
    public static AllConfig? Config { get; private set; } // 静态配置对象，允许访问和设置

    public AllConfig Get() // 获取配置的方法
    {
        lock (_locker) // 确保线程安全
        {
            if (Config == null) // 如果配置为空
            {
                Config = Read(); // 读取配置
                Config.OnAnyChangedAction = Save; // 设置配置更改时的保存操作
                Config.InitEvent(); // 初始化事件
            }

            return Config; // 返回配置对象
        }
    }

    public void Save() // 保存配置的方法
    {
        if (Config != null) // 如果配置不为空
        {
            Write(Config); // 写入配置
        }
    }

    public AllConfig Read() // 读取配置的方法
    {
        _rwLock.EnterReadLock(); // 进入读锁
        try
        {
            var filePath = Global.Absolute(@"User/config.json"); // 获取配置文件路径
            if (!File.Exists(filePath)) // 如果文件不存在
            {
                return new AllConfig(); // 返回新的配置对象
            }

            var json = File.ReadAllText(filePath); // 读取文件内容
            var config = JsonSerializer.Deserialize<AllConfig>(json, JsonOptions); // 反序列化 JSON 为配置对象
            if (config == null) // 如果反序列化结果为空
            {
                return new AllConfig(); // 返回新的配置对象
            }

            Config = config; // 设置静态配置对象
            return config; // 返回配置对象
        }
        catch (Exception e) // 捕获异常
        {
            Console.WriteLine(e.Message); // 输出异常消息
            Console.WriteLine(e.StackTrace); // 输出异常堆栈信息
            return new AllConfig(); // 返回新的配置对象
        }
        finally
        {
            _rwLock.ExitReadLock(); // 退出读锁
        }
    }

    public void Write(AllConfig config) // 写入配置的方法
    # 进入写锁，以确保在写操作期间没有其他线程可以访问资源
    _rwLock.EnterWriteLock();
    try
    {
        # 获取用户目录的绝对路径
        var path = Global.Absolute("User");
        # 如果用户目录不存在，则创建该目录
        if (!Directory.Exists(path))
        {
            Directory.CreateDirectory(path);
        }

        # 构造配置文件的完整路径
        var file = Path.Combine(path, "config.json");
        # 将配置对象序列化为 JSON 格式并写入到配置文件
        File.WriteAllText(file, JsonSerializer.Serialize(config, JsonOptions));
    }
    catch (Exception e)
    {
        # 捕捉到异常时，输出异常消息
        Console.WriteLine(e.Message);
        # 输出异常堆栈信息
        Console.WriteLine(e.StackTrace);
    }
    finally
    {
        # 无论是否发生异常，退出写锁
        _rwLock.ExitWriteLock();
    }
}

# 定义一个将 Rect 类型序列化和反序列化为 JSON 的转换器
public class OpenCvRectJsonConverter : JsonConverter<Rect>
{
    # 从 JSON 读取数据并将其转换为 Rect 类型
    public override unsafe Rect Read(ref Utf8JsonReader reader, Type typeToConvert, JsonSerializerOptions options)
    {
        # 反序列化 JSON 为 RectHelper 对象
        RectHelper helper = JsonSerializer.Deserialize<RectHelper>(ref reader, options);
        # 将 RectHelper 对象转换为 Rect 类型并返回
        return *(Rect*)&helper;
    }

    # 将 Rect 类型序列化为 JSON
    public override unsafe void Write(Utf8JsonWriter writer, Rect value, JsonSerializerOptions options)
    {
        # 将 Rect 对象转换为 RectHelper 对象
        RectHelper helper = *(RectHelper*)&value;
        # 序列化 RectHelper 对象为 JSON
        JsonSerializer.Serialize(writer, helper, options);
    }

    # 不允许修改：保持与 OpenCvSharp.Rect 布局相同
    private struct RectHelper
    {
        public int X { get; set; }    # X 坐标
        public int Y { get; set; }    # Y 坐标
        public int Width { get; set; } # 宽度
        public int Height { get; set; } # 高度
    }
}

# 定义一个将 Point 类型序列化和反序列化为 JSON 的转换器
public class OpenCvPointJsonConverter : JsonConverter<Point>
{
    # 从 JSON 读取数据并将其转换为 Point 类型
    public override unsafe Point Read(ref Utf8JsonReader reader, Type typeToConvert, JsonSerializerOptions options)
    {
        # 反序列化 JSON 为 PointHelper 对象
        PointHelper helper = JsonSerializer.Deserialize<PointHelper>(ref reader, options);
        # 将 PointHelper 对象转换为 Point 类型并返回
        return *(Point*)&helper;
    }

    # 将 Point 类型序列化为 JSON
    public override unsafe void Write(Utf8JsonWriter writer, Point value, JsonSerializerOptions options)
    {
        # 将 Point 对象转换为 PointHelper 对象
        PointHelper helper = *(PointHelper*)&value;
        # 序列化 PointHelper 对象为 JSON
        JsonSerializer.Serialize(writer, helper, options);
    }

    # 不允许修改：保持与 OpenCvSharp.Point 布局相同
    private struct PointHelper
    {
        public int X { get; set; }    # X 坐标
        public int Y { get; set; }    # Y 坐标
    }
}
```