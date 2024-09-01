# `.\better-genshin-impact\Build\MicaSetup\Helper\Device\DeviceHelper.cs`

```cs
# 引入 LINQ 和 Management 命名空间
﻿using System.Linq;
using System.Management;

namespace MicaSetup.Helper;

# 定义一个静态类 DeviceHelper
public static class DeviceHelper
{
    # 计算并返回设备的唯一 ID，基于处理器、BIOS 和主板的序列号
    public static string DeviceID
        => MD5CryptoHelper.ComputeHash($"{ProcessorSerialNumber},{BIOSSerialNumber},{BaseBoardSerialNumber}");

    # 获取处理器的序列号
    public static string ProcessorSerialNumber
        => GetManagementProperty("Win32_Processor", "SerialNumber");

    # 获取 BIOS 的序列号
    public static string BIOSSerialNumber
        => GetManagementProperty("Win32_BIOS", "SerialNumber");

    # 获取主板的序列号
    public static string BaseBoardSerialNumber
        => GetManagementProperty("Win32_BaseBoard", "SerialNumber");

    # 根据指定路径和属性名获取管理属性的值
    private static string GetManagementProperty(string path, string name)
    {
        try
        {
            # 创建 ManagementClass 对象来访问管理对象
            using ManagementClass managementClass = new(path);
            # 获取 ManagementObjectCollection 实例
            using ManagementObjectCollection mn = managementClass.GetInstances();
            # 获取管理类的属性集合
            PropertyDataCollection properties = managementClass.Properties;

            # 遍历属性集合
            foreach (PropertyData property in properties)
            {
                # 如果属性名匹配指定名称
                if (property.Name == name)
                {
                    # 遍历管理对象实例
                    foreach (ManagementObject m in mn.Cast<ManagementObject>())
                    {
                        # 返回匹配属性的值
                        return m.Properties[property.Name].Value.ToString();
                    }
                }
            }
        }
        # 捕获并忽略异常
        catch
        {
        }
        # 如果未找到属性，返回空字符串
        return string.Empty;
    }
}
```