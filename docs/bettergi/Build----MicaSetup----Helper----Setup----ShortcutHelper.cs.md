# `.\better-genshin-impact\Build\MicaSetup\Helper\Setup\ShortcutHelper.cs`

```cs
# 引入必需的命名空间
﻿using System;
using System.IO;
using System.Runtime.InteropServices;
using File = System.IO.File;

namespace MicaSetup.Helper;

# 定义一个静态类 ShortcutHelper，提供快捷方式相关的帮助方法
public static class ShortcutHelper
{
    # 创建一个快捷方式的方法
    public static void CreateShortcut(string directory, string shortcutName, string targetPath, string arguments = null!, string description = null!, string iconLocation = null!)
    {
        # 如果指定的目录不存在，则创建该目录
        if (!Directory.Exists(directory))
        {
            _ = Directory.CreateDirectory(directory);
        }

        # 生成快捷方式的完整路径
        string shortcutPath = Path.Combine(directory, $"{shortcutName}.lnk");

        # 定义动态对象 shell 和 shortcut，初始为 null
        dynamic shell = null!;
        dynamic shortcut = null!;

        try
        {
            # 创建 shell 对象，使用 CLSID 创建 COM 实例
            shell = Activator.CreateInstance(Type.GetTypeFromCLSID(new Guid("72C24DD5-D70A-438B-8A42-98424B88AFB8")));
            # 使用 shell 对象创建快捷方式
            shortcut = shell.CreateShortcut(shortcutPath);
            # 设置快捷方式的目标路径
            shortcut.TargetPath = targetPath;
            # 设置快捷方式的工作目录
            shortcut.WorkingDirectory = Path.GetDirectoryName(targetPath);
            # 设置快捷方式的窗口样式（1 表示普通窗口）
            shortcut.WindowStyle = 1;
            # 设置快捷方式的启动参数
            shortcut.Arguments = arguments;
            # 设置快捷方式的描述
            shortcut.Description = description;
            # 设置快捷方式的图标位置，如果没有指定则使用目标路径作为图标
            shortcut.IconLocation = string.IsNullOrWhiteSpace(iconLocation) ? targetPath : iconLocation;
            # 保存快捷方式
            shortcut.Save();
        }
        finally
        {
            # 确保 COM 对象被释放
            if (shortcut != null)
            {
                _ = Marshal.FinalReleaseComObject(shortcut);
            }
            if (shell != null)
            {
                _ = Marshal.FinalReleaseComObject(shell);
            }
        }
    }

    # 在桌面上创建快捷方式的方法
    public static void CreateShortcutOnDesktop(string shortcutName, string targetPath, string arguments = null!, string description = null!, string iconLocation = null!)
    {
        # 获取桌面的路径
        string desktop = Environment.GetFolderPath(Environment.SpecialFolder.DesktopDirectory);

        # 调用 CreateShortcut 方法在桌面上创建快捷方式
        CreateShortcut(desktop, shortcutName, targetPath, arguments, description, iconLocation);
    }

    # 从桌面上删除快捷方式的方法
    public static void RemoveShortcutOnDesktop(string shortcutName)
    {
        # 获取桌面的路径
        string desktop = Environment.GetFolderPath(Environment.SpecialFolder.DesktopDirectory);
        # 生成快捷方式的完整路径
        string filePath = Path.Combine(desktop, $"{shortcutName}.lnk");

        # 如果文件存在，则删除它
        if (File.Exists(filePath))
        {
            File.Delete(filePath);
        }
    }

    # 在快速启动栏上创建快捷方式的方法（尚未实现）
    public static void CreateShortcutOnQuickLaunch(string shortcutName, string targetPath, string arguments = null!, string description = null!, string iconLocation = null!)
    {
        # 如果操作系统版本是 Windows 10 或更高版本
        if (OsVersionHelper.IsWindows10_OrGreater)
        {
            # 构建快速启动用户固定的隐式应用程序快捷方式路径
            string quickLaunchUserPinnedImplicitAppShortcutsPath = Path.Combine(Environment.GetFolderPath(Environment.SpecialFolder.ApplicationData), @"Microsoft\Internet Explorer\Quick Launch\User Pinned\ImplicitAppShortcuts");
            # 创建快捷方式
            CreateShortcut(quickLaunchUserPinnedImplicitAppShortcutsPath, shortcutName, targetPath, arguments, description, iconLocation);
            # 刷新文件资源管理器，以显示新的快捷方式
            ExplorerHelper.Refresh(quickLaunchUserPinnedImplicitAppShortcutsPath);

            # 构建快速启动用户固定的任务栏快捷方式路径
            string quickLaunchUserPinnedTaskBarPath = Path.Combine(Environment.GetFolderPath(Environment.SpecialFolder.ApplicationData), @"Microsoft\Internet Explorer\Quick Launch\User Pinned\TaskBar");
            # 创建快捷方式
            CreateShortcut(quickLaunchUserPinnedTaskBarPath, shortcutName, targetPath, arguments, description, iconLocation);
            # 刷新文件资源管理器，以显示新的快捷方式
            ExplorerHelper.Refresh(quickLaunchUserPinnedTaskBarPath);
        }
        else
        {
            # 定义动态的 shell 对象
            dynamic shell = null!;

            try
            {
                # 尝试创建 Microsoft Visual C++ 2013 Redistributable 的 shell 对象
                shell = Activator.CreateInstance(Type.GetTypeFromCLSID(new Guid("72C24DD5-D70A-438B-8A42-98424B88AFB8")));
                # 获取快速启动文件夹路径
                string quickLaunchPath = shell.SpecialFolders.Item("Quick Launch");

                # 如果获取不到路径，则构建默认路径
                if (string.IsNullOrWhiteSpace(quickLaunchPath))
                {
                    quickLaunchPath = Path.Combine(Environment.GetFolderPath(Environment.SpecialFolder.ApplicationData), @"Microsoft\Internet Explorer\Quick Launch");
                }
                # 创建快捷方式
                CreateShortcut(quickLaunchPath, shortcutName, targetPath, arguments, description, iconLocation);
            }
            catch (Exception e)
            {
                # 记录异常错误
                Logger.Error(e);
            }
            finally
            {
                # 释放 COM 对象
                if (shell != null)
                {
                    _ = Marshal.FinalReleaseComObject(shell);
                }
            }
        }
    }

    # 静态方法，用于从快速启动中删除快捷方式
    public static void RemoveShortcutOnQuickLaunch(string shortcutName)
    {
        # 判断操作系统是否为 Windows 10 或更高版本
        if (OsVersionHelper.IsWindows10_OrGreater)
        {
            # 生成 Quick Launch 中的 ImplicitAppShortcuts 文件夹路径
            string quickLaunchUserPinnedImplicitAppShortcutsPath = Path.Combine(Environment.GetFolderPath(Environment.SpecialFolder.ApplicationData), @"Microsoft\Internet Explorer\Quick Launch\User Pinned\ImplicitAppShortcuts");
            # 生成 ImplicitAppShortcuts 中快捷方式的完整路径
            string quickLaunchUserPinnedImplicitAppShortcutsLnkPath = Path.Combine(quickLaunchUserPinnedImplicitAppShortcutsPath, $"{shortcutName}.lnk");

            # 如果快捷方式文件存在，则删除
            if (File.Exists(quickLaunchUserPinnedImplicitAppShortcutsLnkPath))
            {
                File.Delete(quickLaunchUserPinnedImplicitAppShortcutsLnkPath);
            }
            # 刷新文件夹视图以反映更改
            ExplorerHelper.Refresh(quickLaunchUserPinnedImplicitAppShortcutsPath);

            # 生成 Quick Launch 中的 TaskBar 文件夹路径
            string quickLaunchUserPinnedTaskBarPath = Path.Combine(Environment.GetFolderPath(Environment.SpecialFolder.ApplicationData), @"Microsoft\Internet Explorer\Quick Launch\User Pinned\TaskBar");
            # 生成 TaskBar 中快捷方式的完整路径
            string quickLaunchUserPinnedTaskBarLnkPath = Path.Combine(quickLaunchUserPinnedTaskBarPath, $"{shortcutName}.lnk");

            # 如果快捷方式文件存在，则删除
            if (File.Exists(quickLaunchUserPinnedTaskBarLnkPath))
            {
                File.Delete(quickLaunchUserPinnedTaskBarLnkPath);
            }
            # 刷新文件夹视图以反映更改
            ExplorerHelper.Refresh(quickLaunchUserPinnedTaskBarPath);
        }
        else
        {
            # 定义一个动态类型的 shell 对象
            dynamic shell = null!;

            try
            {
                # 尝试创建 Microsoft Visual C++ 2013 Redistributable 的 shell 对象
                shell = Activator.CreateInstance(Type.GetTypeFromCLSID(new Guid("72C24DD5-D70A-438B-8A42-98424B88AFB8")));
                # 获取 Quick Launch 文件夹的路径
                string quickLaunchPath = shell.SpecialFolders.Item("Quick Launch");

                # 如果路径为空，则使用默认路径
                if (string.IsNullOrWhiteSpace(quickLaunchPath))
                {
                    quickLaunchPath = Path.Combine(Environment.GetFolderPath(Environment.SpecialFolder.ApplicationData), @"Microsoft\Internet Explorer\Quick Launch");
                }
                # 生成 Quick Launch 中快捷方式的完整路径
                string quickLaunchLnkPath = Path.Combine(quickLaunchPath, $"{shortcutName}.lnk");

                # 如果快捷方式文件存在，则删除
                if (File.Exists(quickLaunchLnkPath))
                {
                    File.Delete(quickLaunchLnkPath);
                }
            }
            catch (Exception e)
            {
                # 捕获异常并记录错误信息
                Logger.Error(e);
            }
            finally
            {
                # 释放 COM 对象
                if (shell != null)
                {
                    _ = Marshal.FinalReleaseComObject(shell);
                }
            }
        }
    }
请提供更多代码或上下文，以便我可以为你添加合适的注释。
```