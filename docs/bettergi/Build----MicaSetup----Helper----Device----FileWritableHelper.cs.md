# `.\better-genshin-impact\Build\MicaSetup\Helper\Device\FileWritableHelper.cs`

```cs
# 引入系统输入输出流的命名空间
﻿using System.IO;

# 定义命名空间 MicaSetup.Helper
namespace MicaSetup.Helper;

# 定义一个静态类 FileWritableHelper
public static class FileWritableHelper
{
    # 定义一个公共静态方法 CheckWritable，用于检查文件是否可写
    public static bool CheckWritable(string fileName)
    {
        # 如果文件不存在，则返回 true（假设文件可写）
        if (!File.Exists(fileName))
        {
            return true;
        }

        try
        {
            # 尝试以追加模式打开文件，获取写入权限，同时允许读写共享
            using FileStream fileStream = new(fileName, FileMode.Append, FileAccess.Write, FileShare.ReadWrite);
            # 返回文件流的 CanWrite 属性，表明是否可以写入文件
            return fileStream.CanWrite;
        }
        catch
        {
            # 捕捉异常并返回 false，表明文件不可写
            return false;
        }
    }
}
```