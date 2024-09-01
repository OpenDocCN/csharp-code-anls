# `.\better-genshin-impact\Build\MicaSetup\Helper\Setup\UninstallHelper.cs`

```cs
# 引用 MicaSetup.Natives 命名空间中的本地函数
using MicaSetup.Natives;
# 引用系统、集合、文件 IO 和 LINQ 库
using System;
using System.Collections.Generic;
using System.IO;
using System.Linq;

# 定义 MicaSetup.Helper 命名空间
namespace MicaSetup.Helper;

# 定义静态类 UninstallHelper
public static class UninstallHelper
{
    # 定义静态方法 Uninstall，接受两个可选的回调参数
    public static void Uninstall(Action<double, string> progressCallback = null!, Action<UninstallReport, object> reportCallback = null!)
    }

    # 定义静态方法 DeleteUninst，用于删除 Uninst.exe 文件
    public static void DeleteUninst()
    {
        # 如果当前选项需要创建 Uninst 文件
        if (Option.Current.IsCreateUninst)
        {
            try
            {
                # 构建 Uninst.exe 文件的完整路径
                string uninstPath = Path.Combine(Option.Current.InstallLocation, "Uninst.exe");
                # 如果文件存在，则删除该文件
                if (File.Exists(uninstPath))
                {
                    File.Delete(uninstPath);
                }
            }
            catch (Exception e)
            {
                # 记录错误信息
                Logger.Error(e);
            }
        }
    }

    # 定义静态方法 DeleteEmptyDirectories，递归删除空目录
    public static void DeleteEmptyDirectories(string rootDir)
    {
        # 获取根目录下的所有子目录
        string[] subDir = Directory.GetDirectories(rootDir);

        # 遍历每个子目录并递归调用 DeleteEmptyDirectories
        foreach (string dir in subDir)
        {
            DeleteEmptyDirectories(dir);
        }

        # 如果目录为空，则删除该目录
        if (!Directory.EnumerateFileSystemEntries(rootDir).Any())
        {
            Directory.Delete(rootDir);
        }
    }

    /// <summary>
    /// <returns>
    /// True => 文件标记为在下次系统启动时删除。
    /// False => 标记文件为在下次系统启动时删除失败。
    /// </returns>
    # 定义静态方法 DeleteDelayUntilReboot，标记文件在下次系统启动时删除
    public static bool DeleteDelayUntilReboot(string filePath)
    {
        # 调用 Kernel32 的 MoveFileEx 函数，标记文件在系统重启时删除
        return Kernel32.MoveFileEx(filePath, null!, MoveFileFlags.MOVEFILE_DELAY_UNTIL_REBOOT);
    }
}

# 定义枚举类型 UninstallReport
public enum UninstallReport
{
    # 枚举值表示任何文件被标记为在下次系统启动时删除
    AnyDeleteDelayUntilReboot,
}
```