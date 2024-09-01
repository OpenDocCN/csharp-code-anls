# `.\better-genshin-impact\Build\MicaSetup\Services\DotNetVersionService.cs`

```cs
﻿using MicaSetup.Helper; // 引入 MicaSetup.Helper 命名空间
using System; // 引入 System 命名空间
using System.IO; // 引入 System.IO 命名空间

namespace MicaSetup.Services; // 定义 MicaSetup.Services 命名空间

public class DotNetVersionService : IDotNetVersionService // 定义 DotNetVersionService 类，实现 IDotNetVersionService 接口
{
    public DotNetInstallInfo GetInfo(Version version, bool offline = false) // 获取指定版本的 .NET 安装信息
    {
        // 对于版本 1.0 抛出未实现异常
        if (version == new Version(1, 0))
        {
            throw new NotImplementedException();
        }
        // 对于版本 1.1 抛出未实现异常
        else if (version == new Version(1, 1))
        {
            throw new NotImplementedException();
        }
        // 对于版本 2.0 抛出未实现异常
        else if (version == new Version(2, 0))
        {
            throw new NotImplementedException();
        }
        // 对于版本 2.1 抛出未实现异常
        else if (version == new Version(2, 1))
        {
            throw new NotImplementedException();
        }
        // 对于版本 2.2 抛出未实现异常
        else if (version == new Version(2, 2))
        {
            throw new NotImplementedException();
        }
        // 对于版本 3.0 抛出未实现异常
        else if (version == new Version(3, 0))
        {
            throw new NotImplementedException();
        }
        // 对于版本 3.1 抛出未实现异常
        else if (version == new Version(3, 1))
        {
            throw new NotImplementedException();
        }
        // 对于版本 5.0 及以上抛出未实现异常
        else if (version >= new Version(5, 0))
        {
            throw new NotImplementedException();
        }
        // 调用 DotNetInstallerHelper 获取指定版本的安装信息
        return DotNetInstallerHelper.GetInfo(version, offline);
    }

    public Version GetNetFrameworkVersion() // 获取 .NET Framework 版本
    {
        // 尝试获取 .NET 4.x 版本
        Version? version = DotNetVersionHelper.GetNet4xVersion();
        if (version != null)
        {
            return version;
        }

        // 尝试获取 .NET 3.x 版本
        version = DotNetVersionHelper.GetNet3xVersion();
        if (version != null)
        {
            return version;
        }

        // 尝试获取 .NET 2.x 版本
        version = DotNetVersionHelper.GetNet2xVersion();
        if (version != null)
        {
            return version;
        }

        // 尝试获取 .NET 1.x 版本
        version = DotNetVersionHelper.GetNet1xVersion();
        if (version != null)
        {
            return version;
        }
        // 如果没有找到版本，返回一个空版本
        return new Version();
    }

    public bool InstallNetFramework(Version version, InstallerProgressChangedEventHandler callback = null!) // 安装指定版本的 .NET Framework
    {
        // 获取指定版本的 .NET 安装信息
        DotNetInstallInfo info = DotNetInstallerHelper.GetInfo(version);

        // 下载 .NET 安装文件
        if (DotNetInstallerHelper.Download(info, callback))
        {
            // 安装 .NET 框架
            bool ret = DotNetInstallerHelper.Install(info, callback);
            // 如果临时文件存在，则删除
            if (File.Exists(info.TempFilePath))
            {
                File.Delete(info.TempFilePath);
            }
            // 返回安装结果
            return ret;
        }
        // 下载失败则返回 false
        return false;
    }

    public Version GetNetCoreVersion() // 获取 .NET Core 版本
    {
        throw new NotImplementedException(); // 方法未实现，抛出未实现异常
    }

    public bool InstallNetCore(Version version, InstallerProgressChangedEventHandler callback = null!) // 安装指定版本的 .NET Core
    {
        throw new NotImplementedException(); // 方法未实现，抛出未实现异常
    }
}
```