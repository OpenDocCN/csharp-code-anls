# `.\better-genshin-impact\Build\MicaSetup\Helper\Setup\PrepareInstallPathHelper.cs`

```cs
﻿using System;

namespace MicaSetup.Helper;

// 定义一个静态类 PrepareInstallPathHelper，用于处理安装路径的准备工作
public static class PrepareInstallPathHelper
{
    // 获取准备安装路径的方法，接受一个键名和一个可选的布尔值
    public static string GetPrepareInstallPath(string keyName, bool preferX86 = false)
    {
        try
        {
            // 从注册表中读取卸载信息
            UninstallInfo info = RegistyUninstallHelper.Read(keyName);

            // 如果安装位置不为空，返回该安装位置
            if (!string.IsNullOrWhiteSpace(info.InstallLocation))
            {
                return info.InstallLocation;
            }
        }
        catch
        {
            // 捕获异常但不处理
        }
        // 如果 preferX86 为 true，返回 32 位程序文件夹路径
        if (preferX86)
        {
            return Environment.GetFolderPath(Environment.SpecialFolder.ProgramFilesX86) + @"\" + Option.Current.KeyName;
        }
        // 否则，返回 64 位程序文件夹路径
        return Environment.GetFolderPath(Environment.SpecialFolder.ProgramFiles) + @"\" + Option.Current.KeyName;
    }
}
```