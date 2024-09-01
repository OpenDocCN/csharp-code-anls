# `.\better-genshin-impact\Build\MicaSetup\Natives\Shell\Dialogs\StockIcons\StockIcons.cs`

```cs
﻿using System; // 引入 System 命名空间，包含基础类和基本功能
using System.Collections.Generic; // 引入集合类库

namespace MicaSetup.Shell.Dialogs; // 定义命名空间 MicaSetup.Shell.Dialogs

public class StockIcons // 定义公共类 StockIcons
{
    private readonly IDictionary<StockIconIdentifier, StockIcon> stockIconCache; // 定义只读字典，存储 StockIconIdentifier 和 StockIcon 的映射
    private readonly StockIconSize defaultSize = StockIconSize.Large; // 定义只读字段，默认图标大小为 Large
    private readonly bool isSelected; // 定义只读字段，指示是否选中
    private readonly bool isLinkOverlay; // 定义只读字段，指示是否是链接叠加

    public StockIcons() // 默认构造函数
    {
        stockIconCache = new Dictionary<StockIconIdentifier, StockIcon>(); // 初始化字典

        var allIdentifiers = Enum.GetValues(typeof(StockIconIdentifier)); // 获取所有 StockIconIdentifier 枚举值

        foreach (StockIconIdentifier id in allIdentifiers) // 遍历所有枚举值
        {
            stockIconCache.Add(id, null!); // 将每个枚举值添加到字典中，初始值为 null
        }
    }

    public StockIcons(StockIconSize size, bool linkOverlay, bool selected) // 带参数的构造函数
    {
        defaultSize = size; // 设置默认图标大小
        isLinkOverlay = linkOverlay; // 设置链接叠加状态
        isSelected = selected; // 设置选中状态

        stockIconCache = new Dictionary<StockIconIdentifier, StockIcon>(); // 初始化字典

        var allIdentifiers = Enum.GetValues(typeof(StockIconIdentifier)); // 获取所有 StockIconIdentifier 枚举值

        foreach (StockIconIdentifier id in allIdentifiers) // 遍历所有枚举值
        {
            stockIconCache.Add(id, null!); // 将每个枚举值添加到字典中，初始值为 null
        }
    }

    public StockIconSize DefaultSize => defaultSize; // 公开只读属性，获取默认图标大小

    public bool DefaultLinkOverlay => isLinkOverlay; // 公开只读属性，获取链接叠加状态

    public bool DefaultSelectedState => isSelected; // 公开只读属性，获取选中状态

    public ICollection<StockIcon> AllStockIcons => GetAllStockIcons(); // 公开只读属性，返回所有图标集合

    public StockIcon DocumentNotAssociated => GetStockIcon(StockIconIdentifier.DocumentNotAssociated); // 公开只读属性，获取未关联文档图标

    public StockIcon DocumentAssociated => GetStockIcon(StockIconIdentifier.DocumentAssociated); // 公开只读属性，获取已关联文档图标

    public StockIcon Application => GetStockIcon(StockIconIdentifier.Application); // 公开只读属性，获取应用程序图标

    public StockIcon Folder => GetStockIcon(StockIconIdentifier.Folder); // 公开只读属性，获取文件夹图标

    public StockIcon FolderOpen => GetStockIcon(StockIconIdentifier.FolderOpen); // 公开只读属性，获取打开的文件夹图标

    public StockIcon Drive525 => GetStockIcon(StockIconIdentifier.Drive525); // 公开只读属性，获取 5.25 英寸驱动器图标

    public StockIcon Drive35 => GetStockIcon(StockIconIdentifier.Drive35); // 公开只读属性，获取 3.5 英寸驱动器图标

    public StockIcon DriveRemove => GetStockIcon(StockIconIdentifier.DriveRemove); // 公开只读属性，获取可移动驱动器图标

    public StockIcon DriveFixed => GetStockIcon(StockIconIdentifier.DriveFixed); // 公开只读属性，获取固定驱动器图标

    public StockIcon DriveNetwork => GetStockIcon(StockIconIdentifier.DriveNetwork); // 公开只读属性，获取网络驱动器图标

    public StockIcon DriveNetworkDisabled => GetStockIcon(StockIconIdentifier.DriveNetworkDisabled); // 公开只读属性，获取禁用网络驱动器图标

    public StockIcon DriveCD => GetStockIcon(StockIconIdentifier.DriveCD); // 公开只读属性，获取 CD 驱动器图标

    public StockIcon DriveRam => GetStockIcon(StockIconIdentifier.DriveRam); // 公开只读属性，获取 RAM 驱动器图标

    public StockIcon World => GetStockIcon(StockIconIdentifier.World); // 公开只读属性，获取世界图标

    public StockIcon Server => GetStockIcon(StockIconIdentifier.Server); // 公开只读属性，获取服务器图标

    public StockIcon Printer => GetStockIcon(StockIconIdentifier.Printer); // 公开只读属性，获取打印机图标

    public StockIcon MyNetwork => GetStockIcon(StockIconIdentifier.MyNetwork); // 公开只读属性，获取我的网络图标

    public StockIcon Find => GetStockIcon(StockIconIdentifier.Find); // 公开只读属性，获取查找图标

    public StockIcon Help => GetStockIcon(StockIconIdentifier.Help); // 公开只读属性，获取帮助图标

    public StockIcon Share => GetStockIcon(StockIconIdentifier.Share); // 公开只读属性，获取共享图标
}
    # 获取表示链接的图标
    public StockIcon Link => GetStockIcon(StockIconIdentifier.Link);

    # 获取表示慢速文件的图标
    public StockIcon SlowFile => GetStockIcon(StockIconIdentifier.SlowFile);

    # 获取表示回收站的图标
    public StockIcon Recycler => GetStockIcon(StockIconIdentifier.Recycler);

    # 获取表示满的回收站的图标
    public StockIcon RecyclerFull => GetStockIcon(StockIconIdentifier.RecyclerFull);

    # 获取表示音频光盘的图标
    public StockIcon MediaCDAudio => GetStockIcon(StockIconIdentifier.MediaCDAudio);

    # 获取表示锁定的图标
    public StockIcon Lock => GetStockIcon(StockIconIdentifier.Lock);

    # 获取表示自动列表的图标
    public StockIcon AutoList => GetStockIcon(StockIconIdentifier.AutoList);

    # 获取表示网络打印机的图标
    public StockIcon PrinterNet => GetStockIcon(StockIconIdentifier.PrinterNet);

    # 获取表示服务器共享的图标
    public StockIcon ServerShare => GetStockIcon(StockIconIdentifier.ServerShare);

    # 获取表示传真打印机的图标
    public StockIcon PrinterFax => GetStockIcon(StockIconIdentifier.PrinterFax);

    # 获取表示网络传真打印机的图标
    public StockIcon PrinterFaxNet => GetStockIcon(StockIconIdentifier.PrinterFaxNet);

    # 获取表示打印文件的图标
    public StockIcon PrinterFile => GetStockIcon(StockIconIdentifier.PrinterFile);

    # 获取表示堆栈的图标
    public StockIcon Stack => GetStockIcon(StockIconIdentifier.Stack);

    # 获取表示 SVCD 媒体的图标
    public StockIcon MediaSvcd => GetStockIcon(StockIconIdentifier.MediaSvcd);

    # 获取表示压缩文件夹的图标
    public StockIcon StuffedFolder => GetStockIcon(StockIconIdentifier.StuffedFolder);

    # 获取表示未知驱动器的图标
    public StockIcon DriveUnknown => GetStockIcon(StockIconIdentifier.DriveUnknown);

    # 获取表示 DVD 驱动器的图标
    public StockIcon DriveDvd => GetStockIcon(StockIconIdentifier.DriveDvd);

    # 获取表示 DVD 媒体的图标
    public StockIcon MediaDvd => GetStockIcon(StockIconIdentifier.MediaDvd);

    # 获取表示 DVD-RAM 媒体的图标
    public StockIcon MediaDvdRam => GetStockIcon(StockIconIdentifier.MediaDvdRam);

    # 获取表示 DVD-RW 媒体的图标
    public StockIcon MediaDvdRW => GetStockIcon(StockIconIdentifier.MediaDvdRW);

    # 获取表示 DVD-R 媒体的图标
    public StockIcon MediaDvdR => GetStockIcon(StockIconIdentifier.MediaDvdR);

    # 获取表示 DVD-ROM 媒体的图标
    public StockIcon MediaDvdRom => GetStockIcon(StockIconIdentifier.MediaDvdRom);

    # 获取表示 CD 音频的图标
    public StockIcon MediaCDAudioPlus => GetStockIcon(StockIconIdentifier.MediaCDAudioPlus);

    # 获取表示 CD-RW 媒体的图标
    public StockIcon MediaCDRW => GetStockIcon(StockIconIdentifier.MediaCDRW);

    # 获取表示 CD-R 媒体的图标
    public StockIcon MediaCDR => GetStockIcon(StockIconIdentifier.MediaCDR);

    # 获取表示 CD 刻录的图标
    public StockIcon MediaCDBurn => GetStockIcon(StockIconIdentifier.MediaCDBurn);

    # 获取表示空白 CD 的图标
    public StockIcon MediaBlankCD => GetStockIcon(StockIconIdentifier.MediaBlankCD);

    # 获取表示 CD-ROM 媒体的图标
    public StockIcon MediaCDRom => GetStockIcon(StockIconIdentifier.MediaCDRom);

    # 获取表示音频文件的图标
    public StockIcon AudioFiles => GetStockIcon(StockIconIdentifier.AudioFiles);

    # 获取表示图像文件的图标
    public StockIcon ImageFiles => GetStockIcon(StockIconIdentifier.ImageFiles);

    # 获取表示视频文件的图标
    public StockIcon VideoFiles => GetStockIcon(StockIconIdentifier.VideoFiles);

    # 获取表示混合文件的图标
    public StockIcon MixedFiles => GetStockIcon(StockIconIdentifier.MixedFiles);

    # 获取表示文件夹后面的图标
    public StockIcon FolderBack => GetStockIcon(StockIconIdentifier.FolderBack);

    # 获取表示文件夹前面的图标
    public StockIcon FolderFront => GetStockIcon(StockIconIdentifier.FolderFront);

    # 获取表示保护盾的图标
    public StockIcon Shield => GetStockIcon(StockIconIdentifier.Shield);

    # 获取表示警告的图标
    public StockIcon Warning => GetStockIcon(StockIconIdentifier.Warning);

    # 获取表示信息的图标
    public StockIcon Info => GetStockIcon(StockIconIdentifier.Info);
    # 获取表示错误的图标
    public StockIcon Error => GetStockIcon(StockIconIdentifier.Error);

    # 获取表示钥匙的图标
    public StockIcon Key => GetStockIcon(StockIconIdentifier.Key);

    # 获取表示软件的图标
    public StockIcon Software => GetStockIcon(StockIconIdentifier.Software);

    # 获取表示重命名的图标
    public StockIcon Rename => GetStockIcon(StockIconIdentifier.Rename);

    # 获取表示删除的图标
    public StockIcon Delete => GetStockIcon(StockIconIdentifier.Delete);

    # 获取表示音频 DVD 的图标
    public StockIcon MediaAudioDvd => GetStockIcon(StockIconIdentifier.MediaAudioDvd);

    # 获取表示电影 DVD 的图标
    public StockIcon MediaMovieDvd => GetStockIcon(StockIconIdentifier.MediaMovieDvd);

    # 获取表示增强型 CD 的图标
    public StockIcon MediaEnhancedCD => GetStockIcon(StockIconIdentifier.MediaEnhancedCD);

    # 获取表示增强型 DVD 的图标
    public StockIcon MediaEnhancedDvd => GetStockIcon(StockIconIdentifier.MediaEnhancedDvd);

    # 获取表示 HD DVD 的图标
    public StockIcon MediaHDDvd => GetStockIcon(StockIconIdentifier.MediaHDDvd);

    # 获取表示 Blu-ray 的图标
    public StockIcon MediaBluRay => GetStockIcon(StockIconIdentifier.MediaBluRay);

    # 获取表示 VCD 的图标
    public StockIcon MediaVcd => GetStockIcon(StockIconIdentifier.MediaVcd);

    # 获取表示 DVD+R 的图标
    public StockIcon MediaDvdPlusR => GetStockIcon(StockIconIdentifier.MediaDvdPlusR);

    # 获取表示 DVD+RW 的图标
    public StockIcon MediaDvdPlusRW => GetStockIcon(StockIconIdentifier.MediaDvdPlusRW);

    # 获取表示桌面 PC 的图标
    public StockIcon DesktopPC => GetStockIcon(StockIconIdentifier.DesktopPC);

    # 获取表示移动 PC 的图标
    public StockIcon MobilePC => GetStockIcon(StockIconIdentifier.MobilePC);

    # 获取表示用户的图标
    public StockIcon Users => GetStockIcon(StockIconIdentifier.Users);

    # 获取表示 SmartMedia 的图标
    public StockIcon MediaSmartMedia => GetStockIcon(StockIconIdentifier.MediaSmartMedia);

    # 获取表示 CompactFlash 的图标
    public StockIcon MediaCompactFlash => GetStockIcon(StockIconIdentifier.MediaCompactFlash);

    # 获取表示手机的图标
    public StockIcon DeviceCellPhone => GetStockIcon(StockIconIdentifier.DeviceCellPhone);

    # 获取表示相机的图标
    public StockIcon DeviceCamera => GetStockIcon(StockIconIdentifier.DeviceCamera);

    # 获取表示摄像机的图标
    public StockIcon DeviceVideoCamera => GetStockIcon(StockIconIdentifier.DeviceVideoCamera);

    # 获取表示音频播放器的图标
    public StockIcon DeviceAudioPlayer => GetStockIcon(StockIconIdentifier.DeviceAudioPlayer);

    # 获取表示网络连接的图标
    public StockIcon NetworkConnect => GetStockIcon(StockIconIdentifier.NetworkConnect);

    # 获取表示互联网的图标
    public StockIcon Internet => GetStockIcon(StockIconIdentifier.Internet);

    # 获取表示 ZIP 文件的图标
    public StockIcon ZipFile => GetStockIcon(StockIconIdentifier.ZipFile);

    # 获取表示设置的图标
    public StockIcon Settings => GetStockIcon(StockIconIdentifier.Settings);

    # 获取表示 HD DVD 驱动器的图标
    public StockIcon DriveHDDVD => GetStockIcon(StockIconIdentifier.DriveHDDVD);

    # 获取表示 Blu-ray 驱动器的图标
    public StockIcon DriveBluRay => GetStockIcon(StockIconIdentifier.DriveBluRay);

    # 获取表示 HD DVD-ROM 的图标
    public StockIcon MediaHDDVDROM => GetStockIcon(StockIconIdentifier.MediaHDDVDROM);

    # 获取表示 HD DVD-R 的图标
    public StockIcon MediaHDDVDR => GetStockIcon(StockIconIdentifier.MediaHDDVDR);

    # 获取表示 HD DVD-RAM 的图标
    public StockIcon MediaHDDVDRAM => GetStockIcon(StockIconIdentifier.MediaHDDVDRAM);

    # 获取表示 Blu-ray-ROM 的图标
    public StockIcon MediaBluRayROM => GetStockIcon(StockIconIdentifier.MediaBluRayROM);

    # 获取表示 Blu-ray-R 的图标
    public StockIcon MediaBluRayR => GetStockIcon(StockIconIdentifier.MediaBluRayR);

    # 获取表示 Blu-ray-RE 的图标
    public StockIcon MediaBluRayRE => GetStockIcon(StockIconIdentifier.MediaBluRayRE);
    // 公开属性，返回 ClusteredDisk 图标的 StockIcon 对象
    public StockIcon ClusteredDisk => GetStockIcon(StockIconIdentifier.ClusteredDisk);

    // 私有方法，根据 StockIconIdentifier 返回对应的 StockIcon 对象
    private StockIcon GetStockIcon(StockIconIdentifier stockIconIdentifier)
    {
        // 如果缓存中存在对应图标，则直接返回
        if (stockIconCache[stockIconIdentifier] != null)
        {
            return stockIconCache[stockIconIdentifier];
        }
        else
        {
            // 如果缓存中没有图标，则创建新的 StockIcon 对象
            var icon = new StockIcon(stockIconIdentifier, defaultSize, isLinkOverlay, isSelected);

            try
            {
                // 将新创建的图标存入缓存
                stockIconCache[stockIconIdentifier] = icon;
            }
            catch
            {
                // 如果发生异常，则释放图标资源，并重新抛出异常
                icon.Dispose();
                throw;
            }
            // 返回新创建的图标
            return icon;
        }
    }

    // 私有方法，返回所有 StockIcon 对象的集合
    private ICollection<StockIcon> GetAllStockIcons()
    {
        // 创建一个数组，存放缓存中所有 StockIconIdentifier
        var ids = new StockIconIdentifier[stockIconCache.Count];
        // 将缓存中的所有键复制到数组中
        stockIconCache.Keys.CopyTo(ids, 0);

        // 遍历所有的 StockIconIdentifier
        foreach (var id in ids)
        {
            // 如果缓存中不存在对应的图标，则触发加载
            if (stockIconCache[id] == null)
                _ = GetStockIcon(id);
        }

        // 返回缓存中的所有 StockIcon 对象
        return stockIconCache.Values;
    }
}
```