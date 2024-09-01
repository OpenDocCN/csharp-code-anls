# `.\better-genshin-impact\Build\MicaSetup\Natives\Shell\Dialogs\KnownFolders\KnownFolders.cs`

```cs
using MicaSetup.Helper; // 引入 MicaSetup.Helper 命名空间
using System; // 引入 System 命名空间
using System.Collections.Generic; // 引入 System.Collections.Generic 命名空间
using System.Collections.ObjectModel; // 引入 System.Collections.ObjectModel 命名空间
using System.Runtime.InteropServices; // 引入 System.Runtime.InteropServices 命名空间

namespace MicaSetup.Shell.Dialogs; // 定义 MicaSetup.Shell.Dialogs 命名空间

public static class KnownFolders // 定义一个公开的静态类 KnownFolders
{
    public static ICollection<IKnownFolder> All => GetAllFolders(); // 属性返回所有已知文件夹的集合，通过调用 GetAllFolders 方法获取

    public static IKnownFolder CommonOemLinks => GetKnownFolder(FolderIdentifiers.CommonOEMLinks); // 属性获取 CommonOEMLinks 文件夹的 IKnownFolder 对象

    public static IKnownFolder DeviceMetadataStore // 属性获取 DeviceMetadataStore 文件夹的 IKnownFolder 对象
    {
        get
        {
            OsVersionHelper.ThrowIfNotWin7(); // 调用 OsVersionHelper 检查是否在 Windows 7 及以上版本
            return GetKnownFolder(FolderIdentifiers.DeviceMetadataStore); // 返回 DeviceMetadataStore 文件夹的 IKnownFolder 对象
        }
    }

    public static IKnownFolder DocumentsLibrary // 属性获取 DocumentsLibrary 文件夹的 IKnownFolder 对象
    {
        get
        {
            OsVersionHelper.ThrowIfNotWin7(); // 调用 OsVersionHelper 检查是否在 Windows 7 及以上版本
            return GetKnownFolder(FolderIdentifiers.DocumentsLibrary); // 返回 DocumentsLibrary 文件夹的 IKnownFolder 对象
        }
    }

    public static IKnownFolder ImplicitAppShortcuts // 属性获取 ImplicitAppShortcuts 文件夹的 IKnownFolder 对象
    {
        get
        {
            OsVersionHelper.ThrowIfNotWin7(); // 调用 OsVersionHelper 检查是否在 Windows 7 及以上版本
            return GetKnownFolder(FolderIdentifiers.ImplicitAppShortcuts); // 返回 ImplicitAppShortcuts 文件夹的 IKnownFolder 对象
        }
    }

    public static IKnownFolder Libraries // 属性获取 Libraries 文件夹的 IKnownFolder 对象
    {
        get
        {
            OsVersionHelper.ThrowIfNotWin7(); // 调用 OsVersionHelper 检查是否在 Windows 7 及以上版本
            return GetKnownFolder(FolderIdentifiers.Libraries); // 返回 Libraries 文件夹的 IKnownFolder 对象
        }
    }

    public static IKnownFolder MusicLibrary // 属性获取 MusicLibrary 文件夹的 IKnownFolder 对象
    {
        get
        {
            OsVersionHelper.ThrowIfNotWin7(); // 调用 OsVersionHelper 检查是否在 Windows 7 及以上版本
            return GetKnownFolder(FolderIdentifiers.MusicLibrary); // 返回 MusicLibrary 文件夹的 IKnownFolder 对象
        }
    }

    public static IKnownFolder OtherUsers // 属性获取 OtherUsers 文件夹的 IKnownFolder 对象
    {
        get
        {
            OsVersionHelper.ThrowIfNotWin7(); // 调用 OsVersionHelper 检查是否在 Windows 7 及以上版本
            return GetKnownFolder(FolderIdentifiers.OtherUsers); // 返回 OtherUsers 文件夹的 IKnownFolder 对象
        }
    }

    public static IKnownFolder PicturesLibrary // 属性获取 PicturesLibrary 文件夹的 IKnownFolder 对象
    {
        get
        {
            OsVersionHelper.ThrowIfNotWin7(); // 调用 OsVersionHelper 检查是否在 Windows 7 及以上版本
            return GetKnownFolder(FolderIdentifiers.PicturesLibrary); // 返回 PicturesLibrary 文件夹的 IKnownFolder 对象
        }
    }

    public static IKnownFolder PublicRingtones // 属性获取 PublicRingtones 文件夹的 IKnownFolder 对象
    {
        get
        {
            OsVersionHelper.ThrowIfNotWin7(); // 调用 OsVersionHelper 检查是否在 Windows 7 及以上版本
            return GetKnownFolder(FolderIdentifiers.PublicRingtones); // 返回 PublicRingtones 文件夹的 IKnownFolder 对象
        }
    }

    public static IKnownFolder RecordedTVLibrary // 属性获取 RecordedTVLibrary 文件夹的 IKnownFolder 对象
    {
        get
        {
            OsVersionHelper.ThrowIfNotWin7(); // 调用 OsVersionHelper 检查是否在 Windows 7 及以上版本
            return GetKnownFolder(FolderIdentifiers.RecordedTVLibrary); // 返回 RecordedTVLibrary 文件夹的 IKnownFolder 对象
        }
    }

    public static IKnownFolder Ringtones // 属性获取 Ringtones 文件夹的 IKnownFolder 对象
    {
        get
        {
            OsVersionHelper.ThrowIfNotWin7(); // 调用 OsVersionHelper 检查是否在 Windows 7 及以上版本
            return GetKnownFolder(FolderIdentifiers.Ringtones); // 返回 Ringtones 文件夹的 IKnownFolder 对象
        }
    }

    public static IKnownFolder UserPinned // 属性获取 UserPinned 文件夹的 IKnownFolder 对象
    {
        get
        {
            OsVersionHelper.ThrowIfNotWin7(); // 调用 OsVersionHelper 检查是否在 Windows 7 及以上版本
            return GetKnownFolder(FolderIdentifiers.UserPinned); // 返回 UserPinned 文件夹的 IKnownFolder 对象
        }
    }

    public static IKnownFolder UserProgramFiles // 属性获取 UserProgramFiles 文件夹的 IKnownFolder 对象
    {
        get
        {
            OsVersionHelper.ThrowIfNotWin7(); // 调用 OsVersionHelper 检查是否在 Windows 7 及以上版本
            return GetKnownFolder(FolderIdentifiers.UserProgramFiles); // 返回 UserProgramFiles 文件夹的 IKnownFolder 对象
        }
    }

    public static IKnownFolder UserProgramFilesCommon // 属性获取 UserProgramFilesCommon 文件夹的 IKnownFolder 对象
    {
        get
        {
            // 检查当前操作系统是否为 Windows 7
            OsVersionHelper.ThrowIfNotWin7();
            // 返回标识符为 UserProgramFilesCommon 的已知文件夹
            return GetKnownFolder(FolderIdentifiers.UserProgramFilesCommon);
        }
    }

    public static IKnownFolder UsersLibraries
    {
        get
        {
            // 检查当前操作系统是否为 Windows 7
            OsVersionHelper.ThrowIfNotWin7();
            // 返回标识符为 UsersLibraries 的已知文件夹
            return GetKnownFolder(FolderIdentifiers.UsersLibraries);
        }
    }

    public static IKnownFolder VideosLibrary
    {
        get
        {
            // 检查当前操作系统是否为 Windows 7
            OsVersionHelper.ThrowIfNotWin7();
            // 返回标识符为 VideosLibrary 的已知文件夹
            return GetKnownFolder(FolderIdentifiers.VideosLibrary);
        }
    }

    // 通过标识符 AddNewPrograms 获取已知文件夹
    public static IKnownFolder AddNewPrograms => GetKnownFolder(FolderIdentifiers.AddNewPrograms);

    // 通过标识符 AdminTools 获取已知文件夹
    public static IKnownFolder AdminTools => GetKnownFolder(FolderIdentifiers.AdminTools);

    // 通过标识符 AppUpdates 获取已知文件夹
    public static IKnownFolder AppUpdates => GetKnownFolder(FolderIdentifiers.AppUpdates);

    // 通过标识符 CDBurning 获取已知文件夹
    public static IKnownFolder CDBurning => GetKnownFolder(FolderIdentifiers.CDBurning);

    // 通过标识符 ChangeRemovePrograms 获取已知文件夹
    public static IKnownFolder ChangeRemovePrograms => GetKnownFolder(FolderIdentifiers.ChangeRemovePrograms);

    // 通过标识符 CommonAdminTools 获取已知文件夹
    public static IKnownFolder CommonAdminTools => GetKnownFolder(FolderIdentifiers.CommonAdminTools);

    // 通过标识符 CommonPrograms 获取已知文件夹
    public static IKnownFolder CommonPrograms => GetKnownFolder(FolderIdentifiers.CommonPrograms);

    // 通过标识符 CommonStartMenu 获取已知文件夹
    public static IKnownFolder CommonStartMenu => GetKnownFolder(FolderIdentifiers.CommonStartMenu);

    // 通过标识符 CommonStartup 获取已知文件夹
    public static IKnownFolder CommonStartup => GetKnownFolder(FolderIdentifiers.CommonStartup);

    // 通过标识符 CommonTemplates 获取已知文件夹
    public static IKnownFolder CommonTemplates => GetKnownFolder(FolderIdentifiers.CommonTemplates);

    // 通过标识符 Computer 获取已知文件夹
    public static IKnownFolder Computer => GetKnownFolder(
                FolderIdentifiers.Computer);

    // 通过标识符 Conflict 获取已知文件夹
    public static IKnownFolder Conflict => GetKnownFolder(
                FolderIdentifiers.Conflict);

    // 通过标识符 Connections 获取已知文件夹
    public static IKnownFolder Connections => GetKnownFolder(
                FolderIdentifiers.Connections);

    // 通过标识符 Contacts 获取已知文件夹
    public static IKnownFolder Contacts => GetKnownFolder(FolderIdentifiers.Contacts);

    // 通过标识符 ControlPanel 获取已知文件夹
    public static IKnownFolder ControlPanel => GetKnownFolder(
                FolderIdentifiers.ControlPanel);

    // 通过标识符 Cookies 获取已知文件夹
    public static IKnownFolder Cookies => GetKnownFolder(FolderIdentifiers.Cookies);

    // 通过标识符 Desktop 获取已知文件夹
    public static IKnownFolder Desktop => GetKnownFolder(
                FolderIdentifiers.Desktop);

    // 通过标识符 Documents 获取已知文件夹
    public static IKnownFolder Documents => GetKnownFolder(FolderIdentifiers.Documents);

    // 通过标识符 Downloads 获取已知文件夹
    public static IKnownFolder Downloads => GetKnownFolder(FolderIdentifiers.Downloads);

    // 通过标识符 Favorites 获取已知文件夹
    public static IKnownFolder Favorites => GetKnownFolder(FolderIdentifiers.Favorites);

    // 通过标识符 Fonts 获取已知文件夹
    public static IKnownFolder Fonts => GetKnownFolder(FolderIdentifiers.Fonts);

    // 通过标识符 Games 获取已知文件夹
    public static IKnownFolder Games => GetKnownFolder(FolderIdentifiers.Games);

    // 通过标识符 GameTasks 获取已知文件夹
    public static IKnownFolder GameTasks => GetKnownFolder(FolderIdentifiers.GameTasks);

    // 通过标识符 History 获取已知文件夹
    public static IKnownFolder History => GetKnownFolder(FolderIdentifiers.History);

    // 通过标识符 Internet 获取已知文件夹
    public static IKnownFolder Internet => GetKnownFolder(
                FolderIdentifiers.Internet);
    # 获取 Internet 缓存文件夹
    public static IKnownFolder InternetCache => GetKnownFolder(FolderIdentifiers.InternetCache);

    # 获取 快捷方式 文件夹
    public static IKnownFolder Links => GetKnownFolder(FolderIdentifiers.Links);

    # 获取 本地应用程序数据文件夹
    public static IKnownFolder LocalAppData => GetKnownFolder(FolderIdentifiers.LocalAppData);

    # 获取 低完整性本地应用程序数据文件夹
    public static IKnownFolder LocalAppDataLow => GetKnownFolder(FolderIdentifiers.LocalAppDataLow);

    # 获取 本地化资源目录
    public static IKnownFolder LocalizedResourcesDir => GetKnownFolder(FolderIdentifiers.LocalizedResourcesDir);

    # 获取 音乐文件夹
    public static IKnownFolder Music => GetKnownFolder(FolderIdentifiers.Music);

    # 获取 网络环境文件夹
    public static IKnownFolder NetHood => GetKnownFolder(FolderIdentifiers.NetHood);

    # 获取 网络文件夹
    public static IKnownFolder Network => GetKnownFolder(
                FolderIdentifiers.Network);

    # 获取 原始图像文件夹
    public static IKnownFolder OriginalImages => GetKnownFolder(FolderIdentifiers.OriginalImages);

    # 获取 照片专辑文件夹
    public static IKnownFolder PhotoAlbums => GetKnownFolder(FolderIdentifiers.PhotoAlbums);

    # 获取 图片文件夹
    public static IKnownFolder Pictures => GetKnownFolder(FolderIdentifiers.Pictures);

    # 获取 播放列表文件夹
    public static IKnownFolder Playlists => GetKnownFolder(FolderIdentifiers.Playlists);

    # 获取 打印机文件夹
    public static IKnownFolder Printers => GetKnownFolder(
                FolderIdentifiers.Printers);

    # 获取 打印机环境文件夹
    public static IKnownFolder PrintHood => GetKnownFolder(FolderIdentifiers.PrintHood);

    # 获取 用户配置文件夹
    public static IKnownFolder Profile => GetKnownFolder(FolderIdentifiers.Profile);

    # 获取 程序数据文件夹
    public static IKnownFolder ProgramData => GetKnownFolder(FolderIdentifiers.ProgramData);

    # 获取 程序文件夹
    public static IKnownFolder ProgramFiles => GetKnownFolder(FolderIdentifiers.ProgramFiles);

    # 获取 公共程序文件夹
    public static IKnownFolder ProgramFilesCommon => GetKnownFolder(FolderIdentifiers.ProgramFilesCommon);

    # 获取 公共程序文件夹 (64位)
    public static IKnownFolder ProgramFilesCommonX64 => GetKnownFolder(FolderIdentifiers.ProgramFilesCommonX64);

    # 获取 公共程序文件夹 (32位)
    public static IKnownFolder ProgramFilesCommonX86 => GetKnownFolder(FolderIdentifiers.ProgramFilesCommonX86);

    # 获取 64位程序文件夹
    public static IKnownFolder ProgramFilesX64 => GetKnownFolder(FolderIdentifiers.ProgramFilesX64);

    # 获取 32位程序文件夹
    public static IKnownFolder ProgramFilesX86 => GetKnownFolder(FolderIdentifiers.ProgramFilesX86);

    # 获取 程序文件夹
    public static IKnownFolder Programs => GetKnownFolder(FolderIdentifiers.Programs);

    # 获取 公共文件夹
    public static IKnownFolder Public => GetKnownFolder(FolderIdentifiers.Public);

    # 获取 公共桌面文件夹
    public static IKnownFolder PublicDesktop => GetKnownFolder(FolderIdentifiers.PublicDesktop);

    # 获取 公共文档文件夹
    public static IKnownFolder PublicDocuments => GetKnownFolder(FolderIdentifiers.PublicDocuments);

    # 获取 公共下载文件夹
    public static IKnownFolder PublicDownloads => GetKnownFolder(FolderIdentifiers.PublicDownloads);

    # 获取 公共游戏任务文件夹
    public static IKnownFolder PublicGameTasks => GetKnownFolder(FolderIdentifiers.PublicGameTasks);

    # 获取 公共音乐文件夹
    public static IKnownFolder PublicMusic => GetKnownFolder(FolderIdentifiers.PublicMusic);

    # 获取 公共图片文件夹
    public static IKnownFolder PublicPictures => GetKnownFolder(FolderIdentifiers.PublicPictures);
    # 获取公共视频文件夹
    public static IKnownFolder PublicVideos => GetKnownFolder(FolderIdentifiers.PublicVideos);

    # 获取快速启动文件夹
    public static IKnownFolder QuickLaunch => GetKnownFolder(FolderIdentifiers.QuickLaunch);

    # 获取最近使用的文件夹
    public static IKnownFolder Recent => GetKnownFolder(FolderIdentifiers.Recent);

    # 获取录制的电视节目文件夹
    public static IKnownFolder RecordedTV => GetKnownFolder(FolderIdentifiers.RecordedTV);

    # 获取回收站文件夹
    public static IKnownFolder RecycleBin => GetKnownFolder(
                FolderIdentifiers.RecycleBin);

    # 获取资源目录文件夹
    public static IKnownFolder ResourceDir => GetKnownFolder(FolderIdentifiers.ResourceDir);

    # 获取漫游应用数据文件夹
    public static IKnownFolder RoamingAppData => GetKnownFolder(FolderIdentifiers.RoamingAppData);

    # 获取示例音乐文件夹
    public static IKnownFolder SampleMusic => GetKnownFolder(FolderIdentifiers.SampleMusic);

    # 获取示例图片文件夹
    public static IKnownFolder SamplePictures => GetKnownFolder(FolderIdentifiers.SamplePictures);

    # 获取示例播放列表文件夹
    public static IKnownFolder SamplePlaylists => GetKnownFolder(FolderIdentifiers.SamplePlaylists);

    # 获取示例视频文件夹
    public static IKnownFolder SampleVideos => GetKnownFolder(FolderIdentifiers.SampleVideos);

    # 获取已保存游戏文件夹
    public static IKnownFolder SavedGames => GetKnownFolder(FolderIdentifiers.SavedGames);

    # 获取已保存搜索文件夹
    public static IKnownFolder SavedSearches => GetKnownFolder(FolderIdentifiers.SavedSearches);

    # 获取搜索 CSC 文件夹
    public static IKnownFolder SearchCsc => GetKnownFolder(FolderIdentifiers.SearchCsc);

    # 获取搜索主页文件夹
    public static IKnownFolder SearchHome => GetKnownFolder(FolderIdentifiers.SearchHome);

    # 获取搜索 MAPI 文件夹
    public static IKnownFolder SearchMapi => GetKnownFolder(FolderIdentifiers.SearchMapi);

    # 获取发送到文件夹
    public static IKnownFolder SendTo => GetKnownFolder(FolderIdentifiers.SendTo);

    # 获取侧边栏默认部件文件夹
    public static IKnownFolder SidebarDefaultParts => GetKnownFolder(FolderIdentifiers.SidebarDefaultParts);

    # 获取侧边栏部件文件夹
    public static IKnownFolder SidebarParts => GetKnownFolder(FolderIdentifiers.SidebarParts);

    # 获取开始菜单文件夹
    public static IKnownFolder StartMenu => GetKnownFolder(FolderIdentifiers.StartMenu);

    # 获取启动文件夹
    public static IKnownFolder Startup => GetKnownFolder(FolderIdentifiers.Startup);

    # 获取同步管理器文件夹
    public static IKnownFolder SyncManager => GetKnownFolder(
                FolderIdentifiers.SyncManager);

    # 获取同步结果文件夹
    public static IKnownFolder SyncResults => GetKnownFolder(
                FolderIdentifiers.SyncResults);

    # 获取同步设置文件夹
    public static IKnownFolder SyncSetup => GetKnownFolder(
                FolderIdentifiers.SyncSetup);

    # 获取系统文件夹
    public static IKnownFolder System => GetKnownFolder(FolderIdentifiers.System);

    # 获取系统 x86 文件夹
    public static IKnownFolder SystemX86 => GetKnownFolder(FolderIdentifiers.SystemX86);

    # 获取模板文件夹
    public static IKnownFolder Templates => GetKnownFolder(FolderIdentifiers.Templates);

    # 获取树属性文件夹
    public static IKnownFolder TreeProperties => GetKnownFolder(FolderIdentifiers.TreeProperties);

    # 获取用户配置文件夹
    public static IKnownFolder UserProfiles => GetKnownFolder(FolderIdentifiers.UserProfiles);

    # 获取用户文件夹
    public static IKnownFolder UsersFiles => GetKnownFolder(FolderIdentifiers.UsersFiles);

    # 获取视频文件夹
    public static IKnownFolder Videos => GetKnownFolder(FolderIdentifiers.Videos);
    // 公开一个只读属性，返回指定标识符的已知文件夹
    public static IKnownFolder Windows => GetKnownFolder(FolderIdentifiers.Windows);

    // 获取所有已知文件夹的只读集合
    private static ReadOnlyCollection<IKnownFolder> GetAllFolders()
    {
        // 创建一个 IKnownFolder 的列表
        IList<IKnownFolder> foldersList = new List<IKnownFolder>();

        // 初始化文件夹的指针
        nint folders = 0;

        try
        {
            // 创建 KnownFolderManager 的实例
            var knownFolderManager = new KnownFolderManagerClass();
            // 获取所有文件夹的 ID 和数量
            knownFolderManager.GetFolderIds(out folders, out var count);

            // 如果有文件夹并且数量大于 0
            if (count > 0 && folders != 0)
            {
                // 遍历所有文件夹 ID
                for (var i = 0; i < count; i++)
                {
                    // 计算当前文件夹 ID 的内存地址
                    var current = new IntPtr((long)folders + (Marshal.SizeOf(typeof(Guid)) * i));

                    // 从内存中读取 GUID
                    var knownFolderID = (Guid)Marshal.PtrToStructure(current, typeof(Guid));

                    // 从已知文件夹 ID 获取 IKnownFolder 实例
                    var kf = KnownFolderHelper.FromKnownFolderIdInternal(knownFolderID);

                    // 如果获取成功，添加到列表中
                    if (kf != null) { foldersList.Add(kf); }
                }
            }
        }
        finally
        {
            // 释放内存
            if (folders != 0) { Marshal.FreeCoTaskMem(folders); }
        }

        // 返回只读的 IKnownFolder 集合
        return new ReadOnlyCollection<IKnownFolder>(foldersList);
    }

    // 根据 GUID 获取已知文件夹
    private static IKnownFolder GetKnownFolder(Guid guid) => KnownFolderHelper.FromKnownFolderId(guid);
你提供的代码是一个单独的右花括号 `}`。请问你需要对这段代码做什么样的注释？
```