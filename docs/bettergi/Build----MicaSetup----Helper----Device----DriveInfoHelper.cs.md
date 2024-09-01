# `.\better-genshin-impact\Build\MicaSetup\Helper\Device\DriveInfoHelper.cs`

```cs
# 引入 System.IO 命名空间以便使用文件和驱动器相关功能
﻿using System.IO;

# 定义命名空间 MicaSetup.Helper
namespace MicaSetup.Helper;

# 定义一个静态类 DriveInfoHelper
public static class DriveInfoHelper
{
    # 定义一个公共静态方法 GetAvailableFreeSpace，接受路径字符串并返回长整型的可用空间
    public static long GetAvailableFreeSpace(string path)
    {
        # 获取路径的根目录（驱动器名）
        string driveName = Path.GetPathRoot(path);
        # 创建 DriveInfo 对象，代表指定的驱动器
        DriveInfo driveInfo = new(driveName);
        # 获取驱动器的可用空间（以字节为单位）
        long availableSpace = driveInfo.AvailableFreeSpace;

        # 返回可用空间的值
        return availableSpace;
    }

    # 定义一个扩展方法 ToFreeSpaceString，用于将长整型的可用空间转换为字符串形式
    public static string ToFreeSpaceString(this long freeSpace)
    {
        # 如果可用空间大于或等于 1GB，转换为 GB 并格式化输出
        if (freeSpace >= 1073741824)
        {
            return $"{(double)freeSpace / 1073741824:0.##}GB";
        }
        # 否则，转换为 MB 并格式化输出
        return $"{(double)freeSpace / 1048576:0.##}MB";
    }
}
```