# `.\better-genshin-impact\Build\MicaSetup\Helper\System\StartMenuHelper.cs`

```cs
# 使用 MicaSetup.Natives 命名空间中的类和方法
using MicaSetup.Natives;
using System;
using System.ComponentModel;
using System.IO;
using System.Runtime.InteropServices;

namespace MicaSetup.Helper;

public static class StartMenuHelper
{
    # 将指定路径的文件添加到“最近使用的文件”列表
    public static void AddToRecent(string targetPath)
    {
        # 调用 Windows API 将文件添加到“最近使用的文件”列表
        Shell32.SHAddToRecentDocs(SHARD.SHARD_PATHW, targetPath);
        # 检查上一个 Windows API 调用是否发生错误
        if (Marshal.GetLastWin32Error() != 0)
        {
            # 如果发生错误，抛出异常
            throw new Win32Exception(nameof(Shell32.SHAddToRecentDocs));
        }
    }

    # 创建开始菜单文件夹，并可选择创建卸载快捷方式
    public static void CreateStartMenuFolder(string folderName, string targetPath, bool isCreateUninst = true)
    {
        # 获取开始菜单的公共应用数据路径
        string startMenuPath = Path.Combine(Environment.GetFolderPath(Environment.SpecialFolder.CommonApplicationData), @"Microsoft\Windows\Start Menu\Programs");
        # 拼接形成新的文件夹路径
        string startMenuFolderPath = Path.Combine(startMenuPath, folderName);

        # 如果目标文件夹不存在，则创建它
        if (!Directory.Exists(startMenuFolderPath))
        {
            Directory.CreateDirectory(startMenuFolderPath);
        }
        # 创建目标路径的快捷方式
        ShortcutHelper.CreateShortcut(startMenuFolderPath, folderName, targetPath);
        # 将创建的快捷方式添加到“最近使用的文件”列表
        AddToRecent(Path.Combine(startMenuFolderPath, $"{folderName}.lnk"));

        # 如果需要，创建卸载程序的快捷方式
        if (isCreateUninst)
        {
            # 拼接形成卸载程序的路径
            string uninstTargetPath = Path.Combine($"{new FileInfo(targetPath).Directory.FullName}", "Uninst.exe");
            # 创建卸载程序的快捷方式
            ShortcutHelper.CreateShortcut(startMenuFolderPath, $"Uninstall_{folderName}", uninstTargetPath);
        }
    }

    # 删除开始菜单中的指定文件夹
    public static void RemoveStartMenuFolder(string folderName)
    {
        # 获取开始菜单的公共应用数据路径
        string startMenuPath = Path.Combine(Environment.GetFolderPath(Environment.SpecialFolder.CommonApplicationData), @"Microsoft\Windows\Start Menu\Programs");
        # 拼接形成目标文件夹路径
        string startMenuFolderPath = Path.Combine(startMenuPath, folderName);

        # 如果目标文件夹存在，则删除它
        if (Directory.Exists(startMenuFolderPath))
        {
            Directory.Delete(startMenuFolderPath, true);
        }
    }
}
```