# `.\better-genshin-impact\Build\MicaSetup\Natives\Shell\Dialogs\Interop\KnownFolders\KnownFoldersCOMGuids.cs`

```cs
# 命名空间声明，组织相关类
﻿namespace MicaSetup.Shell.Dialogs;

# 定义一个内部静态类，存储已知文件夹的 CLSID GUID 常量
internal static class KnownFoldersCLSIDGuid
{
    # 定义已知文件夹管理器的 CLSID GUID
    internal const string KnownFolderManager = "4df0c730-df9d-4ae3-9153-aa6b82e9795a";
}

# 定义一个内部静态类，存储已知文件夹的 IID GUID 常量
internal static class KnownFoldersIIDGuid
{
    # 定义 IKnownFolder 接口的 IID GUID
    internal const string IKnownFolder = "3AA7AF7E-9B36-420c-A8E3-F77D4674A488";
    # 定义 IKnownFolderManager 接口的 IID GUID
    internal const string IKnownFolderManager = "8BE2D872-86AA-4d47-B776-32CCA40C7018";
}

# 定义一个内部静态类，存储已知文件夹的 KFID GUID 常量
internal static class KnownFoldersKFIDGuid
{
    # 定义计算机文件夹的 KFID GUID
    internal const string ComputerFolder = "0AC0837C-BBF8-452A-850D-79D08E667CA7";
    # 定义文档文件夹的 KFID GUID
    internal const string Documents = "FDD39AD0-238F-46AF-ADB4-6C85480369C7";
    # 定义收藏夹文件夹的 KFID GUID
    internal const string Favorites = "1777F761-68AD-4D8A-87BD-30B759FA33DD";
    # 定义个人文件夹的 KFID GUID
    internal const string Profile = "5E6C858F-0E22-4760-9AFE-EA3317B67173";
}
```