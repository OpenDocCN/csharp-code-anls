# `.\better-genshin-impact\Build\MicaSetup\Natives\Shell\Dialogs\StockIcons\StockIconEnums.cs`

```cs
# 定义图标大小的枚举
public enum StockIconSize
{
    # 小图标
    Small,
    # 大图标
    Large,
    # 系统定义的大小
    ShellSize,
}

# 定义图标标识符的枚举，包含不同类型的图标
public enum StockIconIdentifier
{
    # 文档未关联图标
    DocumentNotAssociated = 0,
    # 文档已关联图标
    DocumentAssociated = 1,
    # 应用程序图标
    Application = 2,
    # 文件夹图标
    Folder = 3,
    # 打开的文件夹图标
    FolderOpen = 4,
    # 5.25 英寸驱动器图标
    Drive525 = 5,
    # 3.5 英寸驱动器图标
    Drive35 = 6,
    # 移动驱动器图标
    DriveRemove = 7,
    # 固态驱动器图标
    DriveFixed = 8,
    # 网络驱动器图标
    DriveNetwork = 9,
    # 禁用的网络驱动器图标
    DriveNetworkDisabled = 10,
    # CD 驱动器图标
    DriveCD = 11,
    # RAM 驱动器图标
    DriveRam = 12,
    # 全球图标
    World = 13,
    # 服务器图标
    Server = 15,
    # 打印机图标
    Printer = 16,
    # 我的网络图标
    MyNetwork = 17,
    # 查找图标
    Find = 22,
    # 帮助图标
    Help = 23,
    # 共享图标
    Share = 28,
    # 链接图标
    Link = 29,
    # 慢文件图标
    SlowFile = 30,
    # 回收站图标
    Recycler = 31,
    # 已满的回收站图标
    RecyclerFull = 32,
    # CD 音频媒体图标
    MediaCDAudio = 40,
    # 锁图标
    Lock = 47,
    # 自动列表图标
    AutoList = 49,
    # 网络打印机图标
    PrinterNet = 50,
    # 服务器共享图标
    ServerShare = 51,
    # 传真打印机图标
    PrinterFax = 52,
    # 网络传真打印机图标
    PrinterFaxNet = 53,
    # 打印机文件图标
    PrinterFile = 54,
    # 堆栈图标
    Stack = 55,
    # SVCD 媒体图标
    MediaSvcd = 56,
    # 压缩文件夹图标
    StuffedFolder = 57,
    # 未知驱动器图标
    DriveUnknown = 58,
    # DVD 驱动器图标
    DriveDvd = 59,
    # DVD 媒体图标
    MediaDvd = 60,
    # DVD RAM 媒体图标
    MediaDvdRam = 61,
    # DVD-R 媒体图标
    MediaDvdRW = 62,
    # DVD-R 媒体图标
    MediaDvdR = 63,
    # DVD-ROM 媒体图标
    MediaDvdRom = 64,
    # CD 音频增强图标
    MediaCDAudioPlus = 65,
    # CD-RW 媒体图标
    MediaCDRW = 66,
    # CD-R 媒体图标
    MediaCDR = 67,
    # CD 烧录图标
    MediaCDBurn = 68,
    # 空白 CD 媒体图标
    MediaBlankCD = 69,
    # CD-ROM 媒体图标
    MediaCDRom = 70,
    # 音频文件图标
    AudioFiles = 71,
    # 图像文件图标
    ImageFiles = 72,
    # 视频文件图标
    VideoFiles = 73,
    # 混合文件图标
    MixedFiles = 74,
    # 文件夹背面图标
    FolderBack = 75,
    # 文件夹正面图标
    FolderFront = 76,
    # 盾牌图标
    Shield = 77,
    # 警告图标
    Warning = 78,
    # 信息图标
    Info = 79,
    # 错误图标
    Error = 80,
    # 密钥图标
    Key = 81,
    # 软件图标
    Software = 82,
    # 重命名图标
    Rename = 83,
    # 删除图标
    Delete = 84,
    # 音频 DVD 媒体图标
    MediaAudioDvd = 85,
    # 电影 DVD 媒体图标
    MediaMovieDvd = 86,
    # 增强型 CD 媒体图标
    MediaEnhancedCD = 87,
    # 增强型 DVD 媒体图标
    MediaEnhancedDvd = 88,
    # HDDVD 媒体图标
    MediaHDDvd = 89,
    # 蓝光媒体图标
    MediaBluRay = 90,
    # VCD 媒体图标
    MediaVcd = 91,
    # DVD+R 媒体图标
    MediaDvdPlusR = 92,
    # DVD+RW 媒体图标
    MediaDvdPlusRW = 93,
    # 桌面 PC 图标
    DesktopPC = 94,
    # 移动 PC 图标
    MobilePC = 95,
    # 用户图标
    Users = 96,
    # SmartMedia 媒体图标
    MediaSmartMedia = 97,
    # CompactFlash 媒体图标
    MediaCompactFlash = 98,
    # 手机设备图标
    DeviceCellPhone = 99,
    # 相机设备图标
    DeviceCamera = 100,
    # 视频摄像机设备图标
    DeviceVideoCamera = 101,
    # 音频播放器设备图标
    DeviceAudioPlayer = 102,
    # 网络连接图标
    NetworkConnect = 103,
    # 互联网图标
    Internet = 104,
    # ZIP 文件图标
    ZipFile = 105,
    # 设置图标
    Settings = 106,
    # HDDVD 驱动器图标
    DriveHDDVD = 132,
    # 蓝光驱动器图标
    DriveBluRay = 133,
    # HDDVD 媒体 ROM 图标
    MediaHDDVDROM = 134,
    # HDDVD 媒体 R 图标
    MediaHDDVDR = 135,
    # HDDVD 媒体 RAM 图标
    MediaHDDVDRAM = 136,
    # 蓝光 ROM 媒体图标
    MediaBluRayROM = 137,
    # 蓝光 R 媒体图标
    MediaBluRayR = 138,
    # 蓝光 RE 媒体图标
    MediaBluRayRE = 139,
    # 集群磁盘图标
    ClusteredDisk = 140,
}
```