# `.\better-genshin-impact\Build\MicaSetup\Option.cs`

```cs
﻿using MicaSetup.Helper;  // 引用 MicaSetup.Helper 命名空间
using System.ComponentModel;  // 引用 System.ComponentModel 命名空间

namespace MicaSetup  // 定义 MicaSetup 命名空间
{
    /// <summary>
    /// Option Context  // 描述类的功能：选项上下文
    /// </summary>
    public class Option  // 定义 Option 类
    {
        // 定义一个静态只读属性 Current，返回 Option 类的新实例
        public static Option Current { get; } = new();

        /// <summary>
        /// Indicates whether enable logger  // 描述属性的功能：指示是否启用日志记录
        /// </summary>
        [Category("GlobalSetting")]  // 指定该属性属于 GlobalSetting 类别
        public bool Logging { get; internal set; } = false;  // 定义一个内部设置的布尔属性，初始值为 false

        /// <summary>
        /// Indicates whether App installing  // 描述属性的功能：指示应用程序是否正在安装
        /// </summary>
        [Category("GlobalVariable")]  // 指定该属性属于 GlobalVariable 类别
        public bool Installing { get; set; } = false;  // 定义一个公共布尔属性，初始值为 false

        /// <summary>
        /// Indicates whether App uninstalling  // 描述属性的功能：指示应用程序是否正在卸载
        /// </summary>
        [Category("GlobalVariable")]  // 指定该属性属于 GlobalVariable 类别
        public bool Uninstalling { get; set; } = false;  // 定义一个公共布尔属性，初始值为 false

        /// <summary>
        /// Indicates whether this assembly as uninst  // 描述属性的功能：指示此程序集是否作为卸载程序
        /// </summary>
        [Category("GlobalSetting")]  // 指定该属性属于 GlobalSetting 类别
        public bool IsUninst { get; set; } = false;  // 定义一个公共布尔属性，初始值为 false

        /// <summary>
        /// Indicates whether create uninst after app installed  // 描述属性的功能：指示是否在应用程序安装后创建卸载程序
        /// </summary>
        [Category("GlobalSetting")]  // 指定该属性属于 GlobalSetting 类别
        public bool IsCreateUninst { get; set; } = true;  // 定义一个公共布尔属性，初始值为 true

        /// <summary>
        /// Indicates whether to generate Desktop Shortcut  // 描述属性的功能：指示是否生成桌面快捷方式
        /// </summary>
        [Category("GlobalSetting")]  // 指定该属性属于 GlobalSetting 类别
        public bool IsCreateDesktopShortcut { get; set; } = true;  // 定义一个公共布尔属性，初始值为 true

        /// <summary>
        /// Indicates whether to generate Registry Keys  // 描述属性的功能：指示是否生成注册表键
        /// </summary>
        [Category("GlobalSetting")]  // 指定该属性属于 GlobalSetting 类别
        public bool IsCreateRegistryKeys { get; set; } = true;  // 定义一个公共布尔属性，初始值为 true

        /// <summary>
        /// Indicates whether to generate StartMenu Shortcut  // 描述属性的功能：指示是否生成开始菜单快捷方式
        /// </summary>
        [Category("GlobalSetting")]  // 指定该属性属于 GlobalSetting 类别
        public bool IsCreateStartMenu { get; set; } = true;  // 定义一个公共布尔属性，初始值为 true

        /// <summary>
        /// Indicates whether to generate StartMenu Shortcut  // 描述属性的功能：指示是否生成快速启动快捷方式
        /// </summary>
        [Category("GlobalSetting")]  // 指定该属性属于 GlobalSetting 类别
        public bool IsCreateQuickLaunch { get; set; } = false;  // 定义一个公共布尔属性，初始值为 false

        /// <summary>
        /// Indicates whether to generate AutoRun  // 描述属性的功能：指示是否生成自动运行
        /// </summary>
        [Category("GlobalSetting")]  // 指定该属性属于 GlobalSetting 类别
        public bool IsCreateAsAutoRun { get; set; } = false;  // 定义一个公共布尔属性，初始值为 false

        /// <summary>
        /// Indicates whether to show customize option of AutoRun  // 描述属性的功能：指示是否显示自定义自动运行选项
        /// </summary>
        [Category("GlobalSetting")]  // 指定该属性属于 GlobalSetting 类别
        public bool IsCustomizeVisiableAutoRun { get; set; } = false;  // 定义一个公共布尔属性，初始值为 false

        /// <summary>
        /// Indicates AutoRun CLI  // 描述属性的功能：指示自动运行命令行
        /// </summary>
        [Category("GlobalSetting")]  // 指定该属性属于 GlobalSetting 类别
        public string AutoRunLaunchCommand { get; set; } = string.Empty;  // 定义一个公共字符串属性，初始值为空字符串

        /// <summary>
        /// Prefer to provide classic type folder selector  // 描述属性的功能：偏好使用经典类型文件夹选择器
        /// </summary>
        [Category("GlobalSetting")]  // 指定该属性属于 GlobalSetting 类别
        public bool UseFolderPickerPreferClassic { get; set; } = false;  // 定义一个公共布尔属性，初始值为 false

        /// <summary>
        /// Prefer to provide x86 type install path  // 描述属性的功能：偏好使用 x86 类型的安装路径
        /// </summary>
        [Category("GlobalSetting")]  // 指定该属性属于 GlobalSetting 类别
        public bool UseInstallPathPreferX86 { get; set; } = false;  // 定义一个公共布尔属性，初始值为 false

        /// <summary>
        /// Prefer to provide x86 type Registry Key  // 描述属性的功能：偏好使用 x86 类型的注册表键
        /// </summary>
        [Category("GlobalSetting")]  // 指定该属性属于 GlobalSetting 类别
        public bool? IsUseRegistryPreferX86 { get; set; } = null!;  // 定义一个可空布尔属性，初始值为 null
    # 指示是否允许完整文件夹安全设置
    [Category("GlobalSetting")]
    public bool IsAllowFullFolderSecurity { get; set; } = true;

    # 指示是否允许网络防火墙
    [Category("GlobalSetting")]
    public bool IsAllowFirewall { get; set; } = true;

    # 指示是否刷新资源管理器
    [Category("GlobalSetting")]
    public bool IsRefreshExplorer { get; set; } = false;

    # 指示是否安装证书文件 (*.cer)
    [Category("GlobalSetting")]
    public bool IsInstallCertificate { get; set; } = false;

    # 指定在覆盖安装时要移除的文件扩展名，格式如 "exe,dll,pdb"
    [Category("GlobalSetting")]
    public string OverlayInstallRemoveExt { get; set; } = string.Empty;

    # 存档文件解包密码
    [Category("GlobalSetting")]
    public string UnpackingPassword { get; set; } = null!;

    # 产品 EXE 文件名
    [Category("GlobalSetting")]
    public string ExeName { get; set; } = null!;

    # 指示是否在卸载时保留数据
    [Category("GlobalVariable")]
    public bool KeepMyData { get; set; } = true;

    # 注册表卸载键名称
    [Category("GlobalSetting")]
    public string KeyName { get; set; } = null!;

    # 注册表卸载 `DisplayName` 键值
    [Category("GlobalSetting")]
    public string DisplayName { get; set; } = null!;

    # 注册表卸载 `DisplayIcon` 键值
    [Category("GlobalSetting")]
    public string DisplayIcon { get; set; } = null!;

    # 注册表卸载 `DisplayVersion` 键值
    [Category("GlobalSetting")]
    public string DisplayVersion { get; set; } = null!;

    # 注册表卸载 `InstallLocation` 键值
    [Category("GlobalVariable")]
    public string InstallLocation { get; set; } = null!;

    # 注册表卸载 `Publisher` 键值
    [Category("GlobalSetting")]
    public string Publisher { get; set; } = null!;

    # 注册表卸载 `UninstallString` 键值
    [Category("GlobalVariable")]
    public string UninstallString { get; set; } = null!;

    # 注册表卸载 `SystemComponent` 键值
    # true(1) => 隐藏
    # false(0) 或 _ => 显示 (默认)
    [Category("GlobalSetting")]
    public bool SystemComponent { get; set; } = false;

    # 提供 {AppName}.exe 以自动运行
    [Category("GlobalSetting")]
    // 定义一个名为 AppName 的公共属性，初始值为 null!
    public string AppName { get; set; } = null!;

    // 提供 SetupName 的公共属性
    // 用于全局设置分类
    /// <summary>
    /// Provide SetupName
    /// </summary>
    [Category("GlobalSetting")]
    public string SetupName { get; set; } = null!;

    // 提供用于 Get-AppxPackage 的包名属性
    // 可以从 MSIX/APPX 文件中的 AppxManifest.xml 中获取
    /// <summary>
    /// Provide Package Name for Get-AppxPackage
    /// You can get it from AppxManifest.xml in your MSIX/APPX type file
    /// </summary>
    [Category("GlobalSetting")]
    public string AppxPackageName { get; set; } = null!;
# 结束当前的代码块或代码段
}
```