# `.\better-genshin-impact\Build\MicaSetup\Helper\System\TempPathForkHelper.cs`

```cs
# 导入必要的命名空间
﻿using System;
using System.IO;

namespace MicaSetup.Helper;

public static class TempPathForkHelper
{
    # 定义一个常量字符串，用于标识命令行参数
    public const string ForkedCli = "forked";

    # 静态方法用于处理临时路径的分叉操作
    public static void Fork()
    {
        # 检查命令行参数中是否包含 "forked"
        if (!CommandLineHelper.Has(ForkedCli))
        {
            # 获取临时路径
            string tempPath = SpecialPathHelper.TempPath;

            # 如果临时路径不存在，则创建该目录
            if (!Directory.Exists(tempPath))
            {
                _ = Directory.CreateDirectory(tempPath);
            }
            # 生成要创建的文件路径，文件名取决于是否为卸载操作
            string forkedFilePath = Path.Combine(tempPath, $"{(Option.Current.IsUninst ? "Uninst" : "Setup")}.exe");

            try
            {
                # 获取当前应用程序的域文件路径
                string domainFilePath = Path.Combine(AppDomain.CurrentDomain.BaseDirectory, AppDomain.CurrentDomain.FriendlyName);
                # 复制域文件到指定的临时文件路径
                File.Copy(domainFilePath, forkedFilePath);
                # 记录文件复制的信息
                Logger.Info($"[UseTempPathFork] Copy domain file from '{domainFilePath}' to '{forkedFilePath}'");
                # 以提升权限的方式重启当前进程，传递新的命令行参数
                RuntimeHelper.RestartAsElevated(forkedFilePath, tempPath, $"{RuntimeHelper.ReArguments()} /{ForkedCli}");
            }
            catch (Exception e)
            {
                # 如果发生异常，记录致命错误
                Logger.Fatal(e);
            }
        }
    }

    # 静态方法用于清理操作
    public static void Clean()
    {
        # 检查命令行参数中是否包含 "forked"
        if (CommandLineHelper.Has(ForkedCli))
        {
            # 获取当前应用程序的基本目录
            string tempPath = AppDomain.CurrentDomain.BaseDirectory;
            # 生成要删除的文件路径，文件名取决于是否为卸载操作
            string filePath = Path.Combine(tempPath, $"{(Option.Current.IsUninst ? "Uninst" : "Setup")}.exe");

            # 删除目录及其延迟，直到重启后
            _ = UninstallHelper.DeleteDelayUntilReboot(tempPath);
            try
            {
                # 使用 PowerShell 删除文件和目录
                FluentProcess.Create()
                    .FileName("powershell.exe")
                    .Arguments(
                        $"""
                            Start-Sleep -s 3;
                            Remove-Item "{filePath}";
                            Remove-Item "{tempPath}";
                        """)
                    .UseShellExecute(false)
                    .CreateNoWindow()
                    .Start()
                    .Forget();
            }
            catch
            {
                # 捕获异常但不进行任何处理
            }
        }
    }
}
```