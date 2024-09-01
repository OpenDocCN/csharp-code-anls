# `.\better-genshin-impact\Build\MicaSetup\Natives\Shell\Dialogs\Common\ShellFileSystemFolder.cs`

```cs
# 引入 System.IO 命名空间，用于文件和目录操作
﻿using System.IO;

# 定义命名空间 MicaSetup.Shell.Dialogs
namespace MicaSetup.Shell.Dialogs;

# 壳文件系统文件夹类，继承自 ShellFolder
public class ShellFileSystemFolder : ShellFolder
{
    # 内部构造函数，不对外公开
    internal ShellFileSystemFolder()
    {
    }

    # 内部构造函数，接受 IShellItem2 对象并初始化 nativeShellItem
    internal ShellFileSystemFolder(IShellItem2 shellItem) => nativeShellItem = shellItem;

    # 属性 Path 返回 ParsingName
    public virtual string Path => ParsingName;

    # 静态方法，从文件夹路径创建 ShellFileSystemFolder 实例
    public static ShellFileSystemFolder FromFolderPath(string path)
    {
        # 获取路径的绝对路径
        var absPath = ShellHelper.GetAbsolutePath(path);

        # 检查目录是否存在，不存在则抛出 DirectoryNotFoundException 异常
        if (!Directory.Exists(absPath))
        {
            throw new DirectoryNotFoundException(
                string.Format(System.Globalization.CultureInfo.InvariantCulture,
                LocalizedMessages.FilePathNotExist, path));
        }

        # 创建 ShellFileSystemFolder 实例
        var folder = new ShellFileSystemFolder();
        try
        {
            # 设置 ParsingName 属性为绝对路径
            folder.ParsingName = absPath;
            # 返回创建的 ShellFileSystemFolder 实例
            return folder;
        }
        catch
        {
            # 如果发生异常，释放资源并重新抛出异常
            folder.Dispose();
            throw;
        }
    }
}
```