# `.\better-genshin-impact\Build\MicaSetup\Helper\System\StartMenuAutoRunHelper.cs`

```cs
# 引入必要的命名空间
﻿using System;
using System.IO;
using System.Windows;

namespace MicaSetup.Helper;

# 定义一个静态类，处理启动菜单的自动运行快捷方式
public static class StartMenuAutoRunHelper
{
    # 定义一个只读属性，获取启动文件夹路径
    public static string StartupFolder => Environment.GetEnvironmentVariable("windir") + @"\..\ProgramData\Microsoft\Windows\Start Menu\Programs\Startup\";

    # 定义一个静态方法，用于启用启动菜单快捷方式
    public static void Enable(string shortcutName, string targetPath, string arguments = null!)
    {
        try
        {
            # 检查启动文件夹是否存在
            if (Directory.Exists(StartupFolder))
            {
                # 调用 ShortcutHelper 创建快捷方式
                ShortcutHelper.CreateShortcut(StartupFolder, shortcutName, targetPath, arguments);
            }
        }
        catch (Exception e)
        {
            # 记录错误信息
            Logger.Error(e);
            # 显示错误信息的消息框
            MessageBox.Show("Create Startup ShortCut error" + "See detail following" + e.ToString());
        }
    }

    # 定义一个静态方法，用于禁用启动菜单快捷方式
    public static void Disable(string shortcutName)
    {
        try
        {
            # 构造快捷方式文件的路径
            string lnk = StartupFolder + shortcutName + ".lnk";

            # 检查快捷方式文件是否存在
            if (File.Exists(lnk))
            {
                # 删除快捷方式文件
                File.Delete(lnk);
            }
        }
        catch (Exception e)
        {
            # 记录错误信息
            Logger.Error(e);
        }
    }

    # 定义一个静态方法，根据 enable 标志设置快捷方式的启用状态
    public static void SetEnabled(bool enable, string shortcutName, string targetPath, string arguments = null!)
    {
        # 根据 enable 标志调用 Enable 或 Disable 方法
        if (enable)
        {
            Enable(shortcutName, targetPath, arguments);
        }
        else
        {
            Disable(shortcutName);
        }
    }
}
```