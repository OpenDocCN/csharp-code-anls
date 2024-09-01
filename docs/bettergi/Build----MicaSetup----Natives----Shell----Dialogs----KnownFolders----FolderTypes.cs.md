# `.\better-genshin-impact\Build\MicaSetup\Natives\Shell\Dialogs\KnownFolders\FolderTypes.cs`

```cs
# 定义用于表示不同文件夹类型的 GUID
internal static class FolderTypes
{
    # 通信文件夹的 GUID
    internal static Guid Communications = new Guid(
        0x91475fe5, 0x586b, 0x4eba, 0x8d, 0x75, 0xd1, 0x74, 0x34, 0xb8, 0xcd, 0xf6);

    # 压缩文件夹的 GUID
    internal static Guid CompressedFolder = new Guid(
         0x80213e82, 0xbcfd, 0x4c4f, 0x88, 0x17, 0xbb, 0x27, 0x60, 0x12, 0x67, 0xa9);

    # 联系人文件夹的 GUID
    internal static Guid Contacts = new Guid(
         0xde2b70ec, 0x9bf7, 0x4a93, 0xbd, 0x3d, 0x24, 0x3f, 0x78, 0x81, 0xd4, 0x92);

    # 控制面板类别的 GUID
    internal static Guid ControlPanelCategory = new Guid(
        0xde4f0660, 0xfa10, 0x4b8f, 0xa4, 0x94, 0x06, 0x8b, 0x20, 0xb2, 0x23, 0x07);

    # 控制面板经典视图的 GUID
    internal static Guid ControlPanelClassic = new Guid(
        0x0c3794f3, 0xb545, 0x43aa, 0xa3, 0x29, 0xc3, 0x74, 0x30, 0xc5, 0x8d, 0x2a);

    # 文档文件夹的 GUID
    internal static Guid Documents = new Guid(
        0x7d49d726, 0x3c21, 0x4f05, 0x99, 0xaa, 0xfd, 0xc2, 0xc9, 0x47, 0x46, 0x56);

    # 游戏文件夹的 GUID
    internal static Guid Games = new Guid(
        0xb689b0d0, 0x76d3, 0x4cbb, 0x87, 0xf7, 0x58, 0x5d, 0x0e, 0x0c, 0xe0, 0x70);

    # 通用库的 GUID
    internal static Guid GenericLibrary = new Guid(
        0x5f4eab9a, 0x6833, 0x4f61, 0x89, 0x9d, 0x31, 0xcf, 0x46, 0x97, 0x9d, 0x49);

    # 通用搜索结果的 GUID
    internal static Guid GenericSearchResults = new Guid(
        0x7fde1a1e, 0x8b31, 0x49a5, 0x93, 0xb8, 0x6b, 0xe1, 0x4c, 0xfa, 0x49, 0x43);

    # 无效的 GUID
    internal static Guid Invalid = new Guid(
        0x57807898, 0x8c4f, 0x4462, 0xbb, 0x63, 0x71, 0x04, 0x23, 0x80, 0xb1, 0x09);

    # 库的 GUID
    internal static Guid Library = new Guid(
         0x4badfc68, 0xc4ac, 0x4716, 0xa0, 0xa0, 0x4d, 0x5d, 0xaa, 0x6b, 0x0f, 0x3e);

    # 音乐文件夹的 GUID
    internal static Guid Music = new Guid(
        0xaf9c03d6, 0x7db9, 0x4a15, 0x94, 0x64, 0x13, 0xbf, 0x9f, 0xb6, 0x9a, 0x2a);

    # 音乐图标的 GUID
    internal static Guid MusicIcons = new Guid(
        0x0b7467fb, 0x84ba, 0x4aae, 0xa0, 0x9b, 0x15, 0xb7, 0x10, 0x97, 0xaf, 0x9e);

    # 网络浏览器的 GUID
    internal static Guid NetworkExplorer = new Guid(
         0x25cc242b, 0x9a7c, 0x4f51, 0x80, 0xe0, 0x7a, 0x29, 0x28, 0xfe, 0xbe, 0x42);

    # 未指定的 GUID
    internal static Guid NotSpecified = new Guid(
        0x5c4f28b5, 0xf869, 0x4e84, 0x8e, 0x60, 0xf1, 0x1d, 0xb9, 0x7c, 0x5c, 0xc7);

    # 打开搜索的 GUID
    internal static Guid OpenSearch = new Guid(
        0x8faf9629, 0x1980, 0x46ff, 0x80, 0x23, 0x9d, 0xce, 0xab, 0x9c, 0x3e, 0xe3);

    # 其他用户的 GUID
    internal static Guid OtherUsers = new Guid(
        0xb337fd00, 0x9dd5, 0x4635, 0xa6, 0xd4, 0xda, 0x33, 0xfd, 0x10, 0x2b, 0x7a);

    # 图片文件夹的 GUID
    internal static Guid Pictures = new Guid(
        0xb3690e58, 0xe961, 0x423b, 0xb6, 0x87, 0x38, 0x6e, 0xbf, 0xd8, 0x32, 0x39);

    # 打印机文件夹的 GUID
    internal static Guid Printers = new Guid(
        0x2c7bbec6, 0xc844, 0x4a0a, 0x91, 0xfa, 0xce, 0xf6, 0xf5, 0x9c, 0xfd, 0xa1);

    # 录制的电视节目文件夹的 GUID
    internal static Guid RecordedTV = new Guid(
        0x5557a28f, 0x5da6, 0x4f83, 0x88, 0x09, 0xc2, 0xc9, 0x8a, 0x11, 0xa6, 0xfa);
    # 定义一个内部静态的 Guid，表示回收站的唯一标识符
    internal static Guid RecycleBin = new Guid(
        0xd6d9e004, 0xcd87, 0x442b, 0x9d, 0x57, 0x5e, 0x0a, 0xeb, 0x4f, 0x6f, 0x72);

    # 定义一个内部静态的 Guid，表示保存游戏的唯一标识符
    internal static Guid SavedGames = new Guid(
        0xd0363307, 0x28cb, 0x4106, 0x9f, 0x23, 0x29, 0x56, 0xe3, 0xe5, 0xe0, 0xe7);

    # 定义一个内部静态的 Guid，表示搜索连接器的唯一标识符
    internal static Guid SearchConnector = new Guid(
        0x982725ee, 0x6f47, 0x479e, 0xb4, 0x47, 0x81, 0x2b, 0xfa, 0x7d, 0x2e, 0x8f);

    # 定义一个内部静态的 Guid，表示搜索的唯一标识符
    internal static Guid Searches = new Guid(
        0x0b0ba2e3, 0x405f, 0x415e, 0xa6, 0xee, 0xca, 0xd6, 0x25, 0x20, 0x78, 0x53);

    # 定义一个内部静态的 Guid，表示软件资源管理器的唯一标识符
    internal static Guid SoftwareExplorer = new Guid(
        0xd674391b, 0x52d9, 0x4e07, 0x83, 0x4e, 0x67, 0xc9, 0x86, 0x10, 0xf3, 0x9d);

    # 定义一个内部静态的 Guid，表示用户文件的唯一标识符
    internal static Guid UserFiles = new Guid(
         0xcd0fc69b, 0x71e2, 0x46e5, 0x96, 0x90, 0x5b, 0xcd, 0x9f, 0x57, 0xaa, 0xb3);

    # 定义一个内部静态的 Guid，表示用户库的唯一标识符
    internal static Guid UsersLibraries = new Guid(
        0xc4d98f09, 0x6124, 0x4fe0, 0x99, 0x42, 0x82, 0x64, 0x16, 0x8, 0x2d, 0xa9);

    # 定义一个内部静态的 Guid，表示视频的唯一标识符
    internal static Guid Videos = new Guid(
        0x5fa96407, 0x7e77, 0x483c, 0xac, 0x93, 0x69, 0x1d, 0x05, 0x85, 0x0d, 0xe8);

    # 定义一个私有的静态只读字典，用于将 Guid 映射到字符串
    private static readonly Dictionary<Guid, string> types;

    # 静态构造函数，用于初始化静态字段。SuppressMessage 特性用于压制性能警告
    [SuppressMessage("Microsoft.Performance", "CA1810:InitializeReferenceTypeStaticFieldsInline")]
    static FolderTypes()
    # 初始化一个字典，将文件夹类型的 GUID 映射到其本地化消息
    {
        types = new Dictionary<Guid, string>
        {
            # 定义 "未指定" 类型的 GUID 和对应的本地化消息
            { NotSpecified, LocalizedMessages.FolderTypeNotSpecified },
            # 定义 "无效" 类型的 GUID 和对应的本地化消息
            { Invalid, LocalizedMessages.FolderTypeInvalid },
            # 定义 "通讯" 类型的 GUID 和对应的本地化消息
            { Communications, LocalizedMessages.FolderTypeCommunications },
            # 定义 "压缩文件夹" 类型的 GUID 和对应的本地化消息
            { CompressedFolder, LocalizedMessages.FolderTypeCompressedFolder },
            # 定义 "联系人" 类型的 GUID 和对应的本地化消息
            { Contacts, LocalizedMessages.FolderTypeContacts },
            # 定义 "控制面板类别" 类型的 GUID 和对应的本地化消息
            { ControlPanelCategory, LocalizedMessages.FolderTypeCategory },
            # 定义 "经典控制面板" 类型的 GUID 和对应的本地化消息
            { ControlPanelClassic, LocalizedMessages.FolderTypeClassic },
            # 定义 "文档" 类型的 GUID 和对应的本地化消息
            { Documents, LocalizedMessages.FolderTypeDocuments },
            # 定义 "游戏" 类型的 GUID 和对应的本地化消息
            { Games, LocalizedMessages.FolderTypeGames },
            # 定义 "通用搜索结果" 类型的 GUID 和对应的本地化消息
            { GenericSearchResults, LocalizedMessages.FolderTypeSearchResults },
            # 定义 "通用库" 类型的 GUID 和对应的本地化消息
            { GenericLibrary, LocalizedMessages.FolderTypeGenericLibrary },
            # 定义 "库" 类型的 GUID 和对应的本地化消息
            { Library, LocalizedMessages.FolderTypeLibrary },
            # 定义 "音乐" 类型的 GUID 和对应的本地化消息
            { Music, LocalizedMessages.FolderTypeMusic },
            # 定义 "音乐图标" 类型的 GUID 和对应的本地化消息
            { MusicIcons, LocalizedMessages.FolderTypeMusicIcons },
            # 定义 "网络资源管理器" 类型的 GUID 和对应的本地化消息
            { NetworkExplorer, LocalizedMessages.FolderTypeNetworkExplorer },
            # 定义 "其他用户" 类型的 GUID 和对应的本地化消息
            { OtherUsers, LocalizedMessages.FolderTypeOtherUsers },
            # 定义 "开放搜索" 类型的 GUID 和对应的本地化消息
            { OpenSearch, LocalizedMessages.FolderTypeOpenSearch },
            # 定义 "图片" 类型的 GUID 和对应的本地化消息
            { Pictures, LocalizedMessages.FolderTypePictures },
            # 定义 "打印机" 类型的 GUID 和对应的本地化消息
            { Printers, LocalizedMessages.FolderTypePrinters },
            # 定义 "回收站" 类型的 GUID 和对应的本地化消息
            { RecycleBin, LocalizedMessages.FolderTypeRecycleBin },
            # 定义 "录制的电视" 类型的 GUID 和对应的本地化消息
            { RecordedTV, LocalizedMessages.FolderTypeRecordedTV },
            # 定义 "软件资源管理器" 类型的 GUID 和对应的本地化消息
            { SoftwareExplorer, LocalizedMessages.FolderTypeSoftwareExplorer },
            # 定义 "已保存的游戏" 类型的 GUID 和对应的本地化消息
            { SavedGames, LocalizedMessages.FolderTypeSavedGames },
            # 定义 "搜索连接器" 类型的 GUID 和对应的本地化消息
            { SearchConnector, LocalizedMessages.FolderTypeSearchConnector },
            # 定义 "搜索" 类型的 GUID 和对应的本地化消息
            { Searches, LocalizedMessages.FolderTypeSearches },
            # 定义 "用户库" 类型的 GUID 和对应的本地化消息
            { UsersLibraries, LocalizedMessages.FolderTypeUserLibraries },
            # 定义 "用户文件" 类型的 GUID 和对应的本地化消息
            { UserFiles, LocalizedMessages.FolderTypeUserFiles },
            # 定义 "视频" 类型的 GUID 和对应的本地化消息
            { Videos, LocalizedMessages.FolderTypeVideos }
        };
    }

    # 定义一个内部静态方法，用于根据 GUID 获取文件夹类型的本地化消息
    internal static string GetFolderType(Guid typeId)
    {
        # 尝试从字典中获取与给定 GUID 对应的文件夹类型，如果存在则返回该类型，否则返回空字符串
        return types.TryGetValue(typeId, out var type) ? type : string.Empty;
    }
请提供需要注释的代码片段，我将帮助你添加注释。
```