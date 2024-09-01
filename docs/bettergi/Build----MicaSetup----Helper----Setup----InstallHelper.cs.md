# `.\better-genshin-impact\Build\MicaSetup\Helper\Setup\InstallHelper.cs`

```cs
# 引入 SharpCompress.Common 命名空间，用于处理压缩文件相关的功能
﻿using SharpCompress.Common;

# 引入 SharpCompress.Readers 命名空间，用于读取压缩文件
using SharpCompress.Readers;

# 引入 System 命名空间，用于基本的系统功能
using System;

# 引入 System.IO 命名空间，用于文件和流的操作
using System.IO;

# 引入 System.Text 命名空间，用于字符串和编码操作
using System.Text;

# 定义 MicaSetup.Helper.Helper 命名空间
namespace MicaSetup.Helper.Helper;

# 定义一个静态类 InstallHelper，包含安装和卸载相关的帮助方法
public static class InstallHelper
{
    # 定义静态方法 Install，用于处理安装过程中的压缩文件流
    public static void Install(Stream archiveStream, Action<double, string> progressCallback = null!)
    }

    # 定义静态方法 CreateUninst，用于创建卸载程序
    public static void CreateUninst(Stream uninstStream)
    {
        # 如果 Option.Current.IsCreateUninst 为真，则执行卸载程序创建逻辑
        if (Option.Current.IsCreateUninst)
        {
            try
            {
                # 使用指定的路径和文件名创建文件流，并指定为创建模式
                using FileStream fileStream = new(Path.Combine(Option.Current.InstallLocation, "Uninst.exe"), FileMode.Create);
                # 将卸载程序流的当前位置移至流的开头
                uninstStream.Seek(0, SeekOrigin.Begin);
                # 复制卸载程序流的内容到文件流中
                uninstStream.CopyTo(fileStream);
            }
            catch (Exception e)
            {
                # 捕获异常并记录错误
                Logger.Error(e);
            }
        }
    }
}
```