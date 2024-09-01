# `.\better-genshin-impact\Build\MicaSetup\Design\Controls\Selection.cs`

```cs
namespace MicaSetup.Design.Controls;  # 定义命名空间

public static class Selection  # 定义静态类 Selection
{
    public const string CeliakeyboardEnassociate = "\xe900";  # 常量，键盘关联图标的十六进制编码
    public const string CeliakeyboardMenuLanguage = "\xe901";  # 常量，菜单语言图标的十六进制编码
    public const string Circle = "\xe902";  # 常量，圆形图标的十六进制编码
    public const string ContactsDelete = "\xe903";  # 常量，删除联系人图标的十六进制编码
    public const string ContactSencryptionCalls = "\xe904";  # 常量，联系人加密通话图标的十六进制编码
    public const string DeviceMateBook = "\xe905";  # 常量，设备 MateBook 图标的十六进制编码
    public const string FilesNameDrive = "\xe906";  # 常量，文件名驱动器图标的十六进制编码
    public const string GalleryCollage = "\xe907";  # 常量，相册拼贴图标的十六进制编码
    public const string GalleryCreate = "\xe908";  # 常量，创建相册图标的十六进制编码
    public const string GalleryMaterialSelectCheckbo = "\xe909";  # 常量，选择材料复选框图标的十六进制编码
    public const string GalleryMoveIn10 = "\xe90a";  # 常量，相册移入图标的十六进制编码
    public const string GalleryMoveOut = "\xe90b";  # 常量，相册移出图标的十六进制编码
    public const string GalleryPhotoEditMore = "\xe90c";  # 常量，更多照片编辑图标的十六进制编码
    public const string GalleryRectify = "\xe90d";  # 常量，相册校正图标的十六进制编码
    public const string GalleryRename = "\xe90e";  # 常量，相册重命名图标的十六进制编码
    public const string GallerySet = "\xe90f";  # 常量，相册设置图标的十六进制编码
    public const string GallerySortOrder = "\xe910";  # 常量，相册排序图标的十六进制编码
    public const string GallerySortReverse = "\xe911";  # 常量，相册排序反向图标的十六进制编码
    public const string IcoMoon = "\xe912";  # 常量，IcoMoon 图标的十六进制编码
    public const string MessageForwarding = "\xe913";  # 常量，消息转发图标的十六进制编码
    public const string MessageTheme = "\xe914";  # 常量，消息主题图标的十六进制编码
    public const string PowerOff = "\xe915";  # 常量，电源关闭图标的十六进制编码
    public const string PublicAdd = "\xe916";  # 常量，公共添加图标的十六进制编码
    public const string PublicAddFilled = "\xe917";  # 常量，公共添加填充图标的十六进制编码
    public const string PublicAddNorm = "\xe918";  # 常量，公共添加标准图标的十六进制编码
    public const string PublicApp = "\xe919";  # 常量，公共应用图标的十六进制编码
    public const string PublicArrowLeft = "\xe91a";  # 常量，公共左箭头图标的十六进制编码
    public const string PublicArrowRight = "\xe91b";  # 常量，公共右箭头图标的十六进制编码
    public const string PublicArrowUp0 = "\xe91c";  # 常量，公共向上箭头图标 0 的十六进制编码
    public const string PublicArrowUp1 = "\xe91d";  # 常量，公共向上箭头图标 1 的十六进制编码
    public const string PublicBack = "\xe91e";  # 常量，公共返回图标的十六进制编码
    public const string PublicBackToTop = "\xe91f";  # 常量，公共返回顶部图标的十六进制编码
    public const string PublicBrightness = "\xe920";  # 常量，公共亮度图标的十六进制编码
    public const string PublicCancel = "\xe921";  # 常量，公共取消图标的十六进制编码
    public const string PublicCancelFilled = "\xe922";  # 常量，公共取消填充图标的十六进制编码
    public const string PublicClose = "\xe923";  # 常量，公共关闭图标的十六进制编码
    public const string PublicCloudDownload = "\xe924";  # 常量，公共云下载图标的十六进制编码
    public const string PublicCloudUpload = "\xe925";  # 常量，公共云上传图标的十六进制编码
    public const string PublicConnection = "\xe926";  # 常量，公共连接图标的十六进制编码
    public const string PublicContacts = "\xe927";  # 常量，公共联系人图标的十六进制编码
    public const string PublicCopy = "\xe928";  # 常量，公共复制图标的十六进制编码
    public const string PublicDelete = "\xe929";  # 常量，公共删除图标的十六进制编码
    public const string PublicDoNotDisturb = "\xe92a";  # 常量，公共勿扰图标的十六进制编码
    public const string PublicDownload = "\xe92b";  # 常量，公共下载图标的十六进制编码
    public const string PublicEdit = "\xe92c";  # 常量，公共编辑图标的十六进制编码
    public const string PublicEmail = "\xe92d";  # 常量，公共邮件图标的十六进制编码
    public const string PublicEmailSend = "\xe92e";  # 常量，公共邮件发送图标的十六进制编码
    public const string PublicEnlarge = "\xe92f";  # 常量，公共放大图标的十六进制编码
    public const string PublicFail = "\xe930";  # 常量，公共失败图标的十六进制编码
    public const string PublicFolder = "\xe931";  # 常量，公共文件夹图标的十六进制编码
    public const string PublicForbid = "\xe932";  # 常量，公共禁止图标的十六进制编码
    public const string PublicHelp = "\xe933";  # 常量，公共帮助图标的十六进制编码
    public const string PublicHighLights = "\xe934";  # 常量，公共高亮图标的十六进制编码
    public const string PublicHome = "\xe935";  # 常量，公共主页图标的十六进制编码
    public const string PublicInputSearch = "\xe936";  # 常量，公共搜索输入图标的十六进制编码
    public const string PublicKeyboard = "\xe937";  # 常量，公共键盘图标的十六进制编码
    public const string PublicListCycle = "\xe938";  # 常量，公共列表循环图标的十六进制编码
    // 声明常量字符串 PubliClock，值为 Unicode 转义序列 \xe939
    public const string PubliClock = "\xe939";
    // 声明常量字符串 PublicMessage，值为 Unicode 转义序列 \xe93a
    public const string PublicMessage = "\xe93a";
    // 声明常量字符串 PublicMobileData，值为 Unicode 转义序列 \xe93b
    public const string PublicMobileData = "\xe93b";
    // 声明常量字符串 PublicMove，值为 Unicode 转义序列 \xe93c
    public const string PublicMove = "\xe93c";
    // 声明常量字符串 PublicNavigation，值为 Unicode 转义序列 \xe93d
    public const string PublicNavigation = "\xe93d";
    // 声明常量字符串 PublicOk，值为 Unicode 转义序列 \xe93e
    public const string PublicOk = "\xe93e";
    // 声明常量字符串 PublicOkFilled，值为 Unicode 转义序列 \xe93f
    public const string PublicOkFilled = "\xe93f";
    // 声明常量字符串 PublicPicture，值为 Unicode 转义序列 \xe940
    public const string PublicPicture = "\xe940";
    // 声明常量字符串 PublicPrinter，值为 Unicode 转义序列 \xe941
    public const string PublicPrinter = "\xe941";
    // 声明常量字符串 PublicQuit，值为 Unicode 转义序列 \xe942
    public const string PublicQuit = "\xe942";
    // 声明常量字符串 PublicRandom，值为 Unicode 转义序列 \xe943
    public const string PublicRandom = "\xe943";
    // 声明常量字符串 PublicReduce，值为 Unicode 转义序列 \xe944
    public const string PublicReduce = "\xe944";
    // 声明常量字符串 PublicRefresh，值为 Unicode 转义序列 \xe945
    public const string PublicRefresh = "\xe945";
    // 声明常量字符串 PublicRemove，值为 Unicode 转义序列 \xe946
    public const string PublicRemove = "\xe946";
    // 声明常量字符串 PublicReset，值为 Unicode 转义序列 \xe947
    public const string PublicReset = "\xe947";
    // 声明常量字符串 PublicRing，值为 Unicode 转义序列 \xe948
    public const string PublicRing = "\xe948";
    // 声明常量字符串 PublicRingOff，值为 Unicode 转义序列 \xe949
    public const string PublicRingOff = "\xe949";
    // 声明常量字符串 PublicRotate，值为 Unicode 转义序列 \xe94a
    public const string PublicRotate = "\xe94a";
    // 声明常量字符串 PublicSave，值为 Unicode 转义序列 \xe94b
    public const string PublicSave = "\xe94b";
    // 声明常量字符串 PublicScan，值为 Unicode 转义序列 \xe94c
    public const string PublicScan = "\xe94c";
    // 声明常量字符串 PublicSearch，值为 Unicode 转义序列 \xe94d
    public const string PublicSearch = "\xe94d";
    // 声明常量字符串 PublicSecurity，值为 Unicode 转义序列 \xe94e
    public const string PublicSecurity = "\xe94e";
    // 声明常量字符串 PublicSend，值为 Unicode 转义序列 \xe94f
    public const string PublicSend = "\xe94f";
    // 声明常量字符串 PublicSettings，值为 Unicode 转义序列 \xe950
    public const string PublicSettings = "\xe950";
    // 声明常量字符串 PublicShare，值为 Unicode 转义序列 \xe951
    public const string PublicShare = "\xe951";
    // 声明常量字符串 PublicSift，值为 Unicode 转义序列 \xe952
    public const string PublicSift = "\xe952";
    // 声明常量字符串 PublicSound，值为 Unicode 转义序列 \xe953
    public const string PublicSound = "\xe953";
    // 声明常量字符串 PublicText，值为 Unicode 转义序列 \xe954
    public const string PublicText = "\xe954";
    // 声明常量字符串 PublicThemes，值为 Unicode 转义序列 \xe955
    public const string PublicThemes = "\xe955";
    // 声明常量字符串 PublicThumbsup，值为 Unicode 转义序列 \xe956
    public const string PublicThumbsup = "\xe956";
    // 声明常量字符串 PublicTodo，值为 Unicode 转义序列 \xe957
    public const string PublicTodo = "\xe957";
    // 声明常量字符串 PublicTopping，值为 Unicode 转义序列 \xe958
    public const string PublicTopping = "\xe958";
    // 声明常量字符串 PublicUnlock，值为 Unicode 转义序列 \xe959
    public const string PublicUnlock = "\xe959";
    // 声明常量字符串 PublicUpgrade，值为 Unicode 转义序列 \xe95a
    public const string PublicUpgrade = "\xe95a";
    // 声明常量字符串 PublicUpload，值为 Unicode 转义序列 \xe95b
    public const string PublicUpload = "\xe95b";
    // 声明常量字符串 PublicVolumeDown，值为 Unicode 转义序列 \xe95c
    public const string PublicVolumeDown = "\xe95c";
    // 声明常量字符串 PublicWirelessProjection，值为 Unicode 转义序列 \xe95d
    public const string PublicWirelessProjection = "\xe95d";
    // 声明常量字符串 PublicWorldClock，值为 Unicode 转义序列 \xe95e
    public const string PublicWorldClock = "\xe95e";
    // 声明常量字符串 QuickReply，值为 Unicode 转义序列 \xe95f
    public const string QuickReply = "\xe95f";
    // 声明常量字符串 Reboot，值为 Unicode 转义序列 \xe960
    public const string Reboot = "\xe960";
    // 声明常量字符串 SearchThings，值为 Unicode 转义序列 \xe961
    public const string SearchThings = "\xe961";
    // 声明常量字符串 Squircle，值为 Unicode 转义序列 \xe962
    public const string Squircle = "\xe962";
请提供需要注释的代码。
```