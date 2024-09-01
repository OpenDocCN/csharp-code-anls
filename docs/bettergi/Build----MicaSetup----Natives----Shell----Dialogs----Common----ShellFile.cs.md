# `.\better-genshin-impact\Build\MicaSetup\Natives\Shell\Dialogs\Common\ShellFile.cs`

```cs
# 引入系统文件处理命名空间
﻿using System.IO;

# 定义 MicaSetup.Shell.Dialogs 命名空间
namespace MicaSetup.Shell.Dialogs;

# 定义 ShellFile 类，继承自 ShellObject 类
public class ShellFile : ShellObject
{
    # SuppressMessage 属性用于抑制在构造函数中调用可重写方法的警告
    [System.Diagnostics.CodeAnalysis.SuppressMessage("Microsoft.Usage", "CA2214:DoNotCallOverridableMethodsInConstructors")]
    # 定义构造函数，接受一个路径作为参数
    internal ShellFile(string path)
    {
        # 获取路径的绝对路径
        var absPath = ShellHelper.GetAbsolutePath(path);

        # 检查指定的文件是否存在
        if (!File.Exists(absPath))
        {
            # 如果文件不存在，抛出文件未找到异常
            throw new FileNotFoundException(
                string.Format(System.Globalization.CultureInfo.InvariantCulture,
                LocalizedMessages.FilePathNotExist, path));
        }

        # 设置 ParsingName 属性为绝对路径
        ParsingName = absPath;
    }

    # 定义另一个构造函数，接受一个 IShellItem2 对象作为参数
    internal ShellFile(IShellItem2 shellItem) => nativeShellItem = shellItem;

    # 定义 Path 属性，返回 ParsingName 属性值
    public virtual string Path => ParsingName;

    # 定义静态方法，根据文件路径创建 ShellFile 对象
    public static ShellFile FromFilePath(string path) => new ShellFile(path);
}
```