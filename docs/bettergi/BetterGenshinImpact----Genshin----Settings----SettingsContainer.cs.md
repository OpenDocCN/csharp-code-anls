# `.\better-genshin-impact\BetterGenshinImpact\Genshin\Settings\SettingsContainer.cs`

```cs
# 使用 BetterGenshinImpact 库，包含各种设置类
using BetterGenshinImpact.GameTask.Common;
using Microsoft.Extensions.Logging;
using Microsoft.Win32;
using System;
using System.Diagnostics;
using System.Runtime.InteropServices;
using System.Text.Json;
using System.Text.Json.Serialization;

namespace BetterGenshinImpact.Genshin.Settings;

# 定义设置容器类
public class SettingsContainer
{
    # 存储从 JSON 解析的主数据
    protected MainJson? data = null;
    # 存储语言设置
    public LanguageSettings? Language;
    # 存储分辨率设置
    public ResolutionSettings? Resolution;
    # 存储输入数据设置
    public InputDataSettings? InputData;
    # 存储覆盖控制器设置
    public OverrideControllerSettings? OverrideController;

    # 构造函数，尝试从注册表读取设置
    public SettingsContainer()
    {
        try
        {
            FromReg();
        }
        catch (Exception e)
        {
            # 记录读取注册表信息出错的调试日志
            TaskControl.Logger.LogDebug(e, "读取原神注册表信息出错");
        }
    }

    # 从注册表中读取设置
    public void FromReg()
    {
        # 获取注册表键，如果获取失败则返回
        if (GenshinRegistry.GetRegistryKey() is not { } hk)
        {
            return;
        }

        # 使用注册表键进行操作
        using (hk)
        {
            # 搜索注册表中的值名
            string value_name = SearchRegistryName(hk);
            # 获取指定值名的注册表值，如果不是字节数组则返回
            if (hk.GetValue(value_name) is not byte[] rawBytes)
            {
                return;
            }

            unsafe
            {
                # 在解析时保持 rawBytes 固定不变
                fixed (byte* ptr = rawBytes)
                {
                    # 将字节数组转换为只读内存切片并解析
                    Parse(MemoryMarshal.CreateReadOnlySpanFromNullTerminated(ptr));
                }
            }
        }
    }

    # 解析从注册表读取的原始配置数据
    private void Parse(ReadOnlySpan<byte> rawCfg)
    {
        try
        {
            # 将 JSON 字符串反序列化为 MainJson 对象
            data = JsonSerializer.Deserialize<MainJson>(rawCfg, new JsonSerializerOptions()
            {
                # 允许使用命名浮点数表示法
                NumberHandling = JsonNumberHandling.AllowNamedFloatingPointLiterals,
            });

            # 如果数据为 null，则返回
            if (data is null)
            {
                return;
            }

            # 使用解析的数据初始化语言设置
            Language = new LanguageSettings(data);
            # 初始化分辨率设置
            Resolution = new ResolutionSettings();
            # 使用解析的数据初始化输入数据设置
            InputData = new InputDataSettings(data);
            # 使用解析的数据初始化覆盖控制器设置
            OverrideController = new OverrideControllerSettings(data);
        }
        catch (Exception e)
        {
            # 输出异常信息到调试器
            Debug.WriteLine(e);
        }
    }

    # 搜索注册表键中的指定值名
    private static string SearchRegistryName(RegistryKey key)
    {
        # 初始化值名为空字符串
        string value_name = string.Empty;
        # 获取所有值名
        string[] names = key.GetValueNames();

        # 遍历值名，寻找包含 "GENERAL_DATA" 的值名
        foreach (string name in names)
        {
            if (name.Contains("GENERAL_DATA"))
            {
                value_name = name;
                break;
            }
        }

        # 如果未找到值名，则抛出异常
        if (value_name == string.Empty)
        {
            throw new ArgumentException(value_name);
        }

        # 返回找到的值名
        return value_name;
    }
}
```