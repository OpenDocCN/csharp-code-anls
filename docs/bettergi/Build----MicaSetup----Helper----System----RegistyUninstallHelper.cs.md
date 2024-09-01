# `.\better-genshin-impact\Build\MicaSetup\Helper\System\RegistyUninstallHelper.cs`

```cs
# 使用 Microsoft.Win32 命名空间和其他基础库
﻿using Microsoft.Win32;
using System;
using System.Collections.Generic;
using System.IO;

namespace MicaSetup.Helper;

#pragma warning disable CS8601

# 定义静态类 RegistyUninstallHelper，用于处理注册表中的卸载信息
public static class RegistyUninstallHelper
{
    # 定义静态方法 Write，用于将卸载信息写入注册表
    public static void Write(UninstallInfo info)
    {
        # 打开本地计算机注册表的指定视图，根据 Option.Current.IsUseRegistryPreferX86 判断视图类型
        using RegistryKey key = RegistryKey.OpenBaseKey(RegistryHive.LocalMachine, Option.Current.IsUseRegistryPreferX86 switch
        {
            true => RegistryView.Registry32,
            false => RegistryView.Registry64,
            null or _ => RegistryView.Default,
        });
        # 创建或打开卸载信息的子键
        using RegistryKey subKey = key.CreateSubKey($@"SOFTWARE\Microsoft\Windows\CurrentVersion\Uninstall\{info.KeyName}");

        # 设置注册表值，如果信息为空则设置为空字符串
        subKey.SetValue("DisplayName", info.DisplayName ?? string.Empty);
        subKey.SetValue("DisplayIcon", info.DisplayIcon ?? string.Empty);
        subKey.SetValue("DisplayVersion", info.DisplayVersion ?? string.Empty);
        subKey.SetValue("InstallLocation", info.InstallLocation ?? string.Empty);
        subKey.SetValue("Publisher", info.Publisher ?? string.Empty);
        subKey.SetValue("UninstallString", info.UninstallString ?? string.Empty);
        subKey.SetValue("UninstallData", info.UninstallData ?? string.Empty);
        # 设置额外的注册表值，用于控制修改和修复选项
        subKey.SetValue("NoModify", 1, RegistryValueKind.DWord);
        subKey.SetValue("NoRepair", 1, RegistryValueKind.DWord);
        # 设置系统组件标志
        subKey.SetValue("SystemComponent", info.SystemComponent ? 1 : 0, RegistryValueKind.DWord);
    }

    # 定义静态方法 Read，用于从注册表中读取卸载信息
    public static UninstallInfo Read(string keyName)
    {
        # 创建 UninstallInfo 对象并设置 KeyName
        UninstallInfo info = new()
        {
            KeyName = keyName,
        };

        # 打开本地计算机注册表的指定视图，根据 Option.Current.IsUseRegistryPreferX86 判断视图类型
        using RegistryKey key = RegistryKey.OpenBaseKey(RegistryHive.LocalMachine, Option.Current.IsUseRegistryPreferX86 switch
        {
            true => RegistryView.Registry32,
            false => RegistryView.Registry64,
            null or _ => RegistryView.Default,
        });
        # 打开指定的卸载信息子键
        using RegistryKey subKey = key.OpenSubKey($@"SOFTWARE\Microsoft\Windows\CurrentVersion\Uninstall\{info.KeyName}");

        # 从子键中读取各个信息值
        info.DisplayName = subKey?.GetValue("DisplayName") as string;
        info.DisplayIcon = subKey?.GetValue("DisplayIcon") as string;
        info.InstallLocation = subKey?.GetValue("InstallLocation") as string;
        info.Publisher = subKey?.GetValue("Publisher") as string;
        info.UninstallString = subKey?.GetValue("UninstallString") as string;
        info.UninstallData = subKey?.GetValue("UninstallData") as string;
        # 返回读取的卸载信息
        return info;
    }

    # 定义静态方法 Delete，用于从注册表中删除卸载信息
    public static void Delete(string keyName)
    
}  # 结束前一个代码块（如前一个类或方法）

public class UninstallInfo
{
    # 表示卸载信息的类
    public string KeyName { get; set; } = null!;  # 注册表键名，默认为 null，且不能为空
    public string DisplayName { get; set; } = null!;  # 显示名称，默认为 null，且不能为空
    public string DisplayIcon { get; set; } = null!;  # 显示图标，默认为 null，且不能为空
    public string DisplayVersion { get; set; } = null!;  # 显示版本，默认为 null，且不能为空
    public string InstallLocation { get; set; } = null!;  # 安装位置，默认为 null，且不能为空
    public string Publisher { get; set; } = null!;  # 发布者，默认为 null，且不能为空
    public string UninstallString { get; set; } = null!;  # 卸载字符串，默认为 null，且不能为空
    public string UninstallData { get; set; } = null!;  # 卸载数据，默认为 null，且不能为空
    public bool SystemComponent { get; set; } = false;  # 是否为系统组件，默认为 false
}

public class UninstallDataInfo
{
    # 表示卸载数据的信息的类
    public string InstallLocation { get; set; } = null!;  # 安装位置，默认为 null，且不能为空
    public string UninstallData { get; set; } = null!;  # 卸载数据，默认为 null，且不能为空

    # 卸载数据的完整路径属性
    public string[] UninstallDataFullPath
    {
        get
        {
            List<string> paths = new();  # 初始化一个字符串列表，用于存储路径

            # 遍历卸载数据中的每个项，将其与安装位置组合成完整路径
            foreach (string uninstallDataCurrent in UninstallData?.Split('|', '\n') ?? Array.Empty<string>())
            {
                paths.Add(Path.Combine(InstallLocation ?? string.Empty, uninstallDataCurrent));  # 将安装位置与当前卸载数据项结合成路径并添加到列表
            }
            return paths.ToArray();  # 返回路径列表的数组
        }
    }

    # 重写 ToString 方法以返回卸载数据的字符串表示
    public override string ToString() => UninstallData;
}
```