# `.\better-genshin-impact\Build\MicaSetup\ViewModels\Inst\MainViewModel.cs`

```cs
# 使用 CommunityToolkit.Mvvm 提供的组件模型和命令
using CommunityToolkit.Mvvm.ComponentModel;
using CommunityToolkit.Mvvm.Input;
# 使用自定义控件和帮助器
using MicaSetup.Design.Controls;
using MicaSetup.Helper;
using MicaSetup.Services;
using MicaSetup.Shell.Dialogs;
# 使用 SharpCompress 进行压缩文件读取
using SharpCompress.Readers;
using System;
using System.IO;
# 使用系统的对话框和结果
using DialogResult = System.Windows.Forms.DialogResult;
using FolderBrowserDialog = System.Windows.Forms.FolderBrowserDialog;

namespace MicaSetup.ViewModels;

# 定义 MainViewModel 类，继承自 ObservableObject，表示视图模型
public partial class MainViewModel : ObservableObject
{
    # 声明一个字符串类型的属性，绑定到 UI
    [ObservableProperty]
    private string installPath = PrepareInstallPathHelper.GetPrepareInstallPath(Option.Current.KeyName, Option.Current.UseInstallPathPreferX86);

    # 当 installPath 属性发生变化时调用的方法
    partial void OnInstallPathChanged(string value)
    {
        try
        {
            # 合并路径并去除末尾的斜杠
            value = Path.Combine(value).TrimEnd('\\', '/');
            # 如果路径以驱动器字母结尾，则添加目录分隔符
            if (value.EndsWith(":"))
            {
                value += Path.DirectorySeparatorChar;
            }
            # 获取路径的可用剩余空间
            availableFreeSpaceLong = DriveInfoHelper.GetAvailableFreeSpace(value);
            # 更新可用空间的显示格式
            AvailableFreeSpace = availableFreeSpaceLong.ToFreeSpaceString();
            # 设置安装位置，若路径结尾不含关键名称，则添加关键名称
            Option.Current.InstallLocation = (value?.EndsWith(Option.Current.KeyName, StringComparison.OrdinalIgnoreCase) ?? false) ? value : Path.Combine(value, Option.Current.KeyName);
            # 记录安装位置的调试信息
            Logger.Debug($"[InstallLocation] {Option.Current.InstallLocation}");
            # 标记路径合法
            IsIllegalPath = false;
        }
        catch
        {
            # 捕获异常时标记路径非法
            IsIllegalPath = true;
        }
    }

    # 声明一个字符串类型的属性，绑定到 UI
    [ObservableProperty]
    private string requestedFreeSpace = null!;

    # 私有字段，用于存储请求的空间大小
    private long requestedFreeSpaceLong = default;

    # 声明一个字符串类型的属性，绑定到 UI
    [ObservableProperty]
    private string availableFreeSpace = null!;

    # 私有字段，用于存储可用空间大小
    private long availableFreeSpaceLong = default;

    # 声明一个布尔类型的属性，绑定到 UI
    [ObservableProperty]
    private bool isIllegalPath = false;

    # 声明一个字符串类型的属性，绑定到 UI
    [ObservableProperty]
    private string licenseInfo = null!;

    # 声明一个布尔类型的属性，绑定到 UI
    [ObservableProperty]
    private bool licenseShown = false;

    # 声明一个布尔类型的属性，绑定到 UI
    [ObservableProperty]
    private bool licenseRead = true;

    # 当 licenseRead 属性发生变化时调用的方法
    partial void OnLicenseReadChanged(bool value)
    {
        # 根据 licenseRead 属性的值设置 canStart
        CanStart = value;
    }

    # 声明一个布尔类型的属性，绑定到 UI
    [ObservableProperty]
    private bool canStart = true;

    # 声明一个布尔类型的属性，绑定到 UI
    [ObservableProperty]
    private bool isElevated = RuntimeHelper.IsElevated;

    # 声明一个布尔类型的属性，绑定到 UI
    [ObservableProperty] 
    private bool isCustomizeVisiableAutoRun = Option.Current.IsCustomizeVisiableAutoRun;

    # 声明一个布尔类型的属性，绑定到 UI
    [ObservableProperty]
    private bool autoRun = Option.Current.IsCreateAsAutoRun;

    # 当 autoRun 属性发生变化时调用的方法
    partial void OnAutoRunChanged(bool value)
    {
        # 更新 Option 对象中的 autoRun 设置
        Option.Current.IsCreateAsAutoRun = value;
    }

    # 声明一个布尔类型的属性，绑定到 UI
    [ObservableProperty]
    private bool desktopShortcut = Option.Current.IsCreateDesktopShortcut;

    # 当 desktopShortcut 属性发生变化时调用的方法
    partial void OnDesktopShortcutChanged(bool value)
    {
        # 更新 Option 对象中的 desktopShortcut 设置
        Option.Current.IsCreateDesktopShortcut = value;
    }

    # MainViewModel 类的构造函数
    public MainViewModel()
    {
        // 获取许可证信息字符串，并将其赋值给 LicenseInfo
        LicenseInfo = ResourceHelper.GetString(ServiceManager.GetService<IMuiLanguageService>().GetLicenseUriString());
        // 从资源中获取压缩文件的流
        using Stream archiveStream = ResourceHelper.GetStream("pack://application:,,,/MicaSetup;component/Resources/Setups/publish.7z");

        // 配置解压缩选项
        ReaderOptions readerOptions = new()
        {
            // 查找头部信息
            LookForHeader = true,
            // 如果解压密码为空，则设置为 null，否则使用提供的密码
            Password = string.IsNullOrEmpty(Option.Current.UnpackingPassword) ? null! : Option.Current.UnpackingPassword,
        };

        // 计算解压缩所需的总空间，如果需要创建卸载程序，则增加额外空间
        requestedFreeSpaceLong = ArchiveFileHelper.TotalUncompressSize(archiveStream, readerOptions) + (Option.Current.IsCreateUninst ? 2048000 : 0);
        // 将所需的空间大小转换为友好的字符串格式
        RequestedFreeSpace = requestedFreeSpaceLong.ToFreeSpaceString();
        // 获取可用的剩余空间
        availableFreeSpaceLong = DriveInfoHelper.GetAvailableFreeSpace(installPath);
        // 将可用的空间大小转换为友好的字符串格式
        AvailableFreeSpace = availableFreeSpaceLong.ToFreeSpaceString();
    }

    // 声明一个布尔属性，用于控制安装路径是否显示
    [ObservableProperty]
    private bool installPathShown = false;

    // RelayCommand 属性用于创建一个可以绑定到 UI 命令的命令
    [RelayCommand]
    private void ShowOrHideInstallPath()
    {
        // 切换安装路径显示状态
        InstallPathShown = !InstallPathShown;
    }

    // RelayCommand 属性用于创建一个可以绑定到 UI 命令的命令
    [RelayCommand]
    private void ShowOrHideLincenseInfo()
    {
        // 切换许可证信息显示状态
        LicenseShown = !LicenseShown;
    }

    // RelayCommand 属性用于创建一个可以绑定到 UI 命令的命令
    [RelayCommand]
    private void SelectFolder()
    {
        // 根据选项选择文件夹选择对话框的类型
        if (Option.Current.UseFolderPickerPreferClassic)
        {
            using FolderBrowserDialog dialog = new()
            {
                // 显示新建文件夹按钮
                ShowNewFolderButton = true,
            };

            // 如果用户选择了文件夹，则更新安装路径
            if (dialog.ShowDialog() == DialogResult.OK)
            {
                string selectedFolder = dialog.SelectedPath;
                Option.Current.InstallLocation = InstallPath = selectedFolder;
            }
        }
        else
        {
            using CommonOpenFileDialog dialog = new()
            {
                // 设置为文件夹选择对话框
                IsFolderPicker = true,
            };

            // 如果用户选择了文件夹，则更新安装路径
            if (dialog.ShowDialog() == CommonFileDialogResult.Ok)
            {
                string selectedFolder = dialog.FileName;
                Option.Current.InstallLocation = InstallPath = selectedFolder;
            }
        }
    }

    // RelayCommand 属性用于创建一个可以绑定到 UI 命令的命令
    [RelayCommand]
    private void StartInstall()
    # 更新安装路径
    OnInstallPathChanged(InstallPath);

    # 如果路径非法，显示提示信息并返回
    if (IsIllegalPath)
    {
        _ = MessageBoxX.Info(UIDispatcherHelper.MainWindow, Mui("IllegalPathTips"));
        return;
    }

    try
    {
        # 如果请求的空闲空间大于或等于可用的空闲空间，显示提示信息并返回
        if (requestedFreeSpaceLong >= availableFreeSpaceLong)
        {
            _ = MessageBoxX.Info(UIDispatcherHelper.MainWindow, Mui("AvailableFreeSpaceInsufficientTips"));
            return;
        }
    }
    catch (Exception e)
    {
        # 记录捕获到的异常错误
        Logger.Error(e);
    }

    try
    {
        # 检查安装路径下的文件是否可写，如果不可写，显示提示信息并返回
        if (!FileWritableHelper.CheckWritable(Path.Combine(InstallPath, Option.Current.ExeName)))
        {
            _ = MessageBoxX.Info(UIDispatcherHelper.MainWindow, Mui("LockedTipsAndExitTry", Option.Current.ExeName));
            return;
        }
    }
    catch (Exception e)
    {
        # 记录捕获到的异常错误
        Logger.Error(e);
    }

    # 跳转到下一步
    Routing.GoToNext();
你希望注释的代码是完整的函数或代码块，还是只是一个特定的代码片段？
```