# `.\better-genshin-impact\Build\MicaSetup\Helper\DotNet\DotNetInstallerHelper.cs`

```cs
# 定义一个静态类 DotNetInstallerHelper，包含处理 .NET 安装信息的辅助方法
public static class DotNetInstallerHelper
{
    # 获取 .NET 安装信息的静态方法，接受 .NET 版本和是否离线安装的标志
    public static DotNetInstallInfo GetInfo(Version version, bool offline = false)
    {
        # 创建一个 DotNetInstallInfo 对象，初始化其属性
        DotNetInstallInfo info = new()
        {
            # 设置是否为离线安装
            Offline = offline,
            # 设置 .NET 版本
            Version = version,
            # 设置 SDK 下载页面的 URL
            SDKsUrl = "https://dotnet.microsoft.com/en-us/download/visual-studio-sdks",
        };

        # 如果版本是 .NET Framework 4.8.1
        if (version == new Version(4, 8, 1))
        {
            # 设置安装信息的名称
            info.Name = $".NET Framework 4.8.1";
            # 根据是否为离线安装选择文件名
            info.FileName = offline switch
            {
                true => "ndp481-x86-x64-allos-enu.exe",
                false => "ndp481-web.exe",
            };
            # 设置临时文件路径，临时文件路径目前为空（需进一步处理）
            info.TempFilePath = Path.Combine();
            # 设置安装参数
            info.Arguments = " /q /norestart /ChainingPackage FullX64Bootstrapper";
            # 根据是否为离线安装选择下载 URL
            info.DownloadUrl = offline switch
            {
                true => "https://download.visualstudio.microsoft.com/download/pr/6f083c7e-bd40-44d4-9e3f-ffba71ec8b09/3951fd5af6098f2c7e8ff5c331a0679c/ndp481-x86-x64-allos-enu.exe",
                false => "https://download.visualstudio.microsoft.com/download/pr/6f083c7e-bd40-44d4-9e3f-ffba71ec8b09/d05099507287c103a91bb68994498bde/ndp481-web.exe",
            };
            # 设置安装后感谢页面的 URL
            info.ThankYouUrl = "https://dotnet.microsoft.com/en-us/download/dotnet-framework/thank-you/net481-web-installer";
            # 设置发布说明页面的 URL
            info.ReleaseNoteUrl = "https://devblogs.microsoft.com/dotnet/announcing-dotnet-framework-481";
        }
        # 如果版本是 .NET Framework 4.8
        else if (version == new Version(4, 8))
        {
            # 设置安装信息的名称
            info.Name = $".NET Framework 4.8";
            # 根据是否为离线安装选择文件名
            info.FileName = offline switch
            {
                true => "ndp48-x86-x64-allos-enu.exe",
                false => "ndp48-web.exe",
            };
            # 设置临时文件路径
            info.TempFilePath = Path.Combine(SpecialPathHelper.TempPath.SureDirectoryExists(), info.FileName);
            # 设置安装参数
            info.Arguments = " /q /norestart /ChainingPackage FullX64Bootstrapper";
            # 根据是否为离线安装选择下载 URL
            info.DownloadUrl = offline switch
            {
                true => "https://download.visualstudio.microsoft.com/download/pr/2d6bb6b2-226a-4baa-bdec-798822606ff1/8494001c276a4b96804cde7829c04d7f/ndp48-x86-x64-allos-enu.exe",
                false => "https://download.visualstudio.microsoft.com/download/pr/2d6bb6b2-226a-4baa-bdec-798822606ff1/9b7b8746971ed51a1770ae4293618187/ndp48-web.exe",
            };
            # 设置安装后感谢页面的 URL
            info.ThankYouUrl = "https://dotnet.microsoft.com/en-us/download/dotnet-framework/thank-you/net48-web-installer";
            # 设置发布说明页面的 URL
            info.ReleaseNoteUrl = "https://devblogs.microsoft.com/dotnet/announcing-the-net-framework-4-8";
        }
        # 如果版本不是上述任何版本，则抛出未实现异常
        else
        {
            throw new NotImplementedException();
        }
        # 返回设置好的 DotNetInstallInfo 对象
        return info;
    }

    # 定义一个静态方法用于下载 .NET 安装信息，接收安装进度回调函数
    public static bool Download(DotNetInstallInfo info, InstallerProgressChangedEventHandler callback = null!)
    # 获取 info.Version，如果未实现则抛出未实现异常
    _ = info.Version ?? throw new NotImplementedException();
    # 获取 info.DownloadUrl，如果未实现则抛出未实现异常
    _ = info.DownloadUrl ?? throw new NotImplementedException();
    # 获取 info.FileName，如果未实现则抛出未实现异常
    _ = info.FileName ?? throw new NotImplementedException();

    # 记录日志，显示 .NET Framework 下载信息
    Logger.Info($"[DotNetInstaller] Download .NET Framework {info.Version} from '{info.DownloadUrl}' and save to '{info.TempFilePath}'.");

    # 检查网络连接是否可用，若不可用则抛出网络不可用异常
    if (!ConnectivityHelper.IsNetworkAvailable || !ConnectivityHelper.Ping())
    {
        throw new Exception($"{Mui("NetworkUnavailableTips")}");
    }
    # 调用回调函数，报告下载进度
    callback?.Invoke(ProgressType.Download, new ProgressChangedEventArgs(0, null!));
    # 调用 SimpleDownloadHelper 下载文件，并在下载过程中调用回调函数报告进度
    return SimpleDownloadHelper.DownloadFile(info.DownloadUrl, info.TempFilePath, (s, e) => callback?.Invoke(ProgressType.Download, e));
}

# 静态方法，用于安装 .NET Framework
public static bool Install(DotNetInstallInfo info, InstallerProgressChangedEventHandler callback = null!)
{
    # 获取 info.Version，如果未实现则抛出未实现异常
    _ = info.Version ?? throw new NotImplementedException();
    # 获取 info.FileName，如果未实现则抛出未实现异常
    _ = info.FileName ?? throw new NotImplementedException();
    # 获取 info.Arguments，如果未实现则抛出未实现异常
    _ = info.Arguments ?? throw new NotImplementedException();

    # 记录日志，显示 .NET Framework 安装信息
    Logger.Info($"[DotNetInstaller] Install .NET Framework {info.Version} from '{info.TempFilePath}'.");

    # 调用回调函数，报告安装开始的进度
    callback?.Invoke(ProgressType.Install, new ProgressChangedEventArgs(-1, null!));
    # 创建进程，启动 .NET Framework 安装程序，等待其退出，并获取退出代码
    int exitCode = FluentProcess.Create()
        .FileName(info.TempFilePath)
        .Arguments(info.Arguments)
        .Start()
        .WaitForExit()
        .ExitCode;
    # 判断安装是否成功
    bool ret = exitCode == 0;

    # 如果安装成功，调用回调函数报告安装完成的进度
    if (ret)
    {
        callback?.Invoke(ProgressType.Install, new ProgressChangedEventArgs(100, null!));
    }
    # 如果安装失败，记录错误日志
    else
    {
        Logger.Error($"[DotNetInstaller] Install .NET Framework {info.Version} exit with {exitCode}.");
    }
    # 返回安装是否成功的结果
    return ret;
}
# 类 DotNetInstallInfo 用于存储 .NET 安装信息
public class DotNetInstallInfo
{
    # .NET 名称
    public string Name { get; set; } = null!;
    # .NET 版本
    public Version Version { get; set; } = null!;
    # SDK 下载链接
    public string SDKsUrl { get; set; } = null!;
    # 感谢页面链接
    public string ThankYouUrl { get; set; } = null!;
    # 下载页面链接
    public string DownloadUrl { get; set; } = null!;
    # 发布说明页面链接
    public string ReleaseNoteUrl { get; set; } = null!;
    # 是否离线安装
    public bool Offline { get; set; } = false;
    # 文件名称
    public string FileName { get; set; } = null!;
    # 临时文件路径
    public string TempFilePath { get; set; } = null!;
    # 安装参数
    public string Arguments { get; set; } = null!;
}

# 委托，用于处理安装进度更新事件
public delegate void InstallerProgressChangedEventHandler(ProgressType type, ProgressChangedEventArgs e);

# 进度类型枚举，包括下载和安装
public enum ProgressType
{
    # 下载进度
    Download,
    # 安装进度
    Install,
}
```