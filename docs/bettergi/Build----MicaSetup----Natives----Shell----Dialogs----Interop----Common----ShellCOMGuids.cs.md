# `.\better-genshin-impact\Build\MicaSetup\Natives\Shell\Dialogs\Interop\Common\ShellCOMGuids.cs`

```cs
namespace MicaSetup.Shell.Dialogs;  # 声明命名空间 MicaSetup.Shell.Dialogs

internal static class ShellBHIDGuid
{
    internal const string ShellFolderObject = "3981e224-f559-11d3-8e3a-00c04f6837d5";  # 壳资源的 GUID
}

internal static class ShellCLSIDGuid
{
    internal const string ConditionFactory = "E03E85B0-7BE3-4000-BA98-6C13DE9FA486";  # 条件工厂的 CLSID
    internal const string FileOpenDialog = "DC1C5A9C-E88A-4DDE-A5A1-60F82A20AEF7";  # 文件打开对话框的 CLSID
    internal const string FileSaveDialog = "C0B4E2F3-BA21-4773-8DBA-335EC946EB8B";  # 文件保存对话框的 CLSID
    internal const string KnownFolderManager = "4DF0C730-DF9D-4AE3-9153-AA6B82E9795A";  # 已知文件夹管理器的 CLSID
    internal const string QueryParserManager = "5088B39A-29B4-4d9d-8245-4EE289222F66";  # 查询解析器管理器的 CLSID
    internal const string SearchFolderItemFactory = "14010e02-bbbd-41f0-88e3-eda371216584";  # 搜索文件夹项工厂的 CLSID
    internal const string ShellLibrary = "D9B3211D-E57F-4426-AAEF-30A806ADD397";  # 壳库的 CLSID
}

internal static class ShellIIDGuid
{
    internal const string CShellLink = "00021401-0000-0000-C000-000000000046";  # Shell 链接接口的 IID
    internal const string ICondition = "0FC988D4-C935-4b97-A973-46282EA175C8";  # 条件接口的 IID
    internal const string IConditionFactory = "A5EFE073-B16F-474f-9F3E-9F8B497A3E08";  # 条件工厂接口的 IID
    internal const string IEnumIDList = "000214F2-0000-0000-C000-000000000046";  # ID 列表枚举接口的 IID
    internal const string IEnumUnknown = "00000100-0000-0000-C000-000000000046";  # 未知枚举接口的 IID
    internal const string IFileDialog = "42F85136-DB7E-439C-85F1-E4075D135FC8";  # 文件对话框接口的 IID
    internal const string IFileDialogControlEvents = "36116642-D713-4B97-9B83-7484A9D00433";  # 文件对话框控件事件接口的 IID
    internal const string IFileDialogCustomize = "E6FDD21A-163F-4975-9C8C-A69F1BA37034";  # 文件对话框自定义接口的 IID
    internal const string IFileDialogEvents = "973510DB-7D7F-452B-8975-74A85828D354";  # 文件对话框事件接口的 IID
    internal const string IFileOpenDialog = "D57C7288-D4AD-4768-BE02-9D969532D960";  # 文件打开对话框接口的 IID
    internal const string IFileSaveDialog = "84BCCD23-5FDE-4CDB-AEA4-AF64B83D78AB";  # 文件保存对话框接口的 IID
    internal const string IModalWindow = "B4DB1657-70D7-485E-8E3E-6FCB5A5C1802";  # 模态窗口接口的 IID
    internal const string IPersist = "0000010c-0000-0000-C000-000000000046";  # 持久化接口的 IID
    internal const string IPersistStream = "00000109-0000-0000-C000-000000000046";  # 持久化流接口的 IID
    internal const string IPropertyDescription = "6F79D558-3E96-4549-A1D1-7D75D2288814";  # 属性描述接口的 IID
    internal const string IPropertyDescription2 = "57D2EDED-5062-400E-B107-5DAE79FE57A6";  # 属性描述接口2的 IID
    internal const string IPropertyDescriptionList = "1F9FC1D0-C39B-4B26-817F-011967D3440E";  # 属性描述列表接口的 IID
    internal const string IPropertyEnumType = "11E1FBF9-2D56-4A6B-8DB3-7CD193A471F2";  # 属性枚举类型接口的 IID
    internal const string IPropertyEnumType2 = "9B6E051C-5DDD-4321-9070-FE2ACB55E794";  # 属性枚举类型接口2的 IID
    internal const string IPropertyEnumTypeList = "A99400F4-3D84-4557-94BA-1242FB2CC9A6";  # 属性枚举类型列表接口的 IID
    internal const string IPropertyStore = "886D8EEB-8CF2-4446-8D02-CDBA1DBDCF99";  # 属性存储接口的 IID
    internal const string IPropertyStoreCache = "3017056d-9a91-4e90-937d-746c72abbf4f";  # 属性存储缓存接口的 IID
    internal const string IPropertyStoreCapabilities = "c8e2d566-186e-4d49-bf41-6909ead56acc";  # 属性存储能力接口的 IID
    internal const string IQueryParser = "2EBDEE67-3505-43f8-9946-EA44ABC8E5B0";  # 查询解析器接口的 IID
    internal const string IQueryParserManager = "A879E3C4-AF77-44fb-8F37-EBD1487CF920";  # 查询解析器管理器接口的 IID
}
    # 定义 IQuerySolution 接口的 GUID
    internal const string IQuerySolution = "D6EBC66B-8921-4193-AFDD-A1789FB7FF57";
    # 定义 IRichChunk 接口的 GUID
    internal const string IRichChunk = "4FDEF69C-DBC9-454e-9910-B34F3C64B510";
    # 定义 ISearchFolderItemFactory 接口的 GUID
    internal const string ISearchFolderItemFactory = "a0ffbc28-5482-4366-be27-3e81e78e06c2";
    # 定义 ISharedBitmap 接口的 GUID
    internal const string ISharedBitmap = "091162a4-bc96-411f-aae8-c5122cd03363";
    # 定义 IShellFolder 接口的 GUID
    internal const string IShellFolder = "000214E6-0000-0000-C000-000000000046";
    # 定义 IShellFolder2 接口的 GUID
    internal const string IShellFolder2 = "93F2F68C-1D1B-11D3-A30E-00C04F79ABD1";
    # 定义 IShellItem 接口的 GUID
    internal const string IShellItem = "43826D1E-E718-42EE-BC55-A1E261C37BFE";
    # 定义 IShellItem2 接口的 GUID
    internal const string IShellItem2 = "7E9FB0D3-919F-4307-AB2E-9B1860310C93";
    # 定义 IShellItemArray 接口的 GUID
    internal const string IShellItemArray = "B63EA76D-1F85-456F-A19C-48159EFA858B";
    # 定义 IShellLibrary 接口的 GUID
    internal const string IShellLibrary = "11A66EFA-382E-451A-9234-1E0E12EF3085";
    # 定义 IShellLinkW 接口的 GUID
    internal const string IShellLinkW = "000214F9-0000-0000-C000-000000000046";
    # 定义 IThumbnailCache 接口的 GUID
    internal const string IThumbnailCache = "F676C15D-596A-4ce2-8234-33996F445DB1";
}
# 定义一个内部静态类 ShellKFIDGuid
internal static class ShellKFIDGuid
{
    # 定义计算机文件夹的 GUID 常量
    internal const string ComputerFolder = "0AC0837C-BBF8-452A-850D-79D08E667CA7";
    # 定义文档文件夹的 GUID 常量
    internal const string Documents = "FDD39AD0-238F-46AF-ADB4-6C85480369C7";
    # 定义文档库的 GUID 常量
    internal const string DocumentsLibrary = "7d49d726-3c21-4f05-99aa-fdc2c9474656";
    # 定义收藏夹的 GUID 常量
    internal const string Favorites = "1777F761-68AD-4D8A-87BD-30B759FA33DD";
    # 定义通用库的 GUID 常量
    internal const string GenericLibrary = "5c4f28b5-f869-4e84-8e60-f11db97c5cc7";
    # 定义库文件夹的 GUID 常量
    internal const string Libraries = "1B3EA5DC-B587-4786-B4EF-BD1DC332AEAE";
    # 定义音乐库的 GUID 常量
    internal const string MusicLibrary = "94d6ddcc-4a68-4175-a374-bd584a510b78";
    # 定义图片库的 GUID 常量
    internal const string PicturesLibrary = "b3690e58-e961-423b-b687-386ebfd83239";
    # 定义个人资料文件夹的 GUID 常量
    internal const string Profile = "5E6C858F-0E22-4760-9AFE-EA3317B67173";
    # 定义视频库的 GUID 常量
    internal const string VideosLibrary = "5fa96407-7e77-483c-ac93-691d05850de8";
}
```