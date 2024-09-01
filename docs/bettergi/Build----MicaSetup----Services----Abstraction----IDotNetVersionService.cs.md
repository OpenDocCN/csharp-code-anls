# `.\better-genshin-impact\Build\MicaSetup\Services\Abstraction\IDotNetVersionService.cs`

```cs
# 引入 MicaSetup.Helper 命名空间，用于在代码中使用相关的帮助类
﻿using MicaSetup.Helper;

# 引入 System 命名空间，提供基础类库功能
using System;

# 定义 MicaSetup.Services 命名空间，组织相关服务类
namespace MicaSetup.Services;

# 定义一个接口 IDotNetVersionService，描述 .NET 版本服务的操作
public interface IDotNetVersionService
{
    # 定义一个方法 GetInfo，接受一个 Version 对象和一个可选的布尔值 offline，返回 DotNetInstallInfo 对象
    public DotNetInstallInfo GetInfo(Version version, bool offline = false);

    # 定义一个方法 GetNetFrameworkVersion，返回当前安装的 .NET Framework 版本
    public Version GetNetFrameworkVersion();

    # 定义一个方法 InstallNetFramework，接受一个 Version 对象和一个可选的 InstallerProgressChangedEventHandler 回调，返回布尔值表示安装是否成功
    public bool InstallNetFramework(Version version, InstallerProgressChangedEventHandler callback = null!);

    # 定义一个方法 GetNetCoreVersion，返回当前安装的 .NET Core 版本
    public Version GetNetCoreVersion();

    # 定义一个方法 InstallNetCore，接受一个 Version 对象和一个可选的 InstallerProgressChangedEventHandler 回调，返回布尔值表示安装是否成功
    public bool InstallNetCore(Version version, InstallerProgressChangedEventHandler callback = null!);
}
```