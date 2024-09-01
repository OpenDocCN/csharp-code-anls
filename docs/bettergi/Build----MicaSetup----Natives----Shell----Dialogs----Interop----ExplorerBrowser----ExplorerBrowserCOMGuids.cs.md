# `.\better-genshin-impact\Build\MicaSetup\Natives\Shell\Dialogs\Interop\ExplorerBrowser\ExplorerBrowserCOMGuids.cs`

```cs
# 定义 MicaSetup.Shell.Dialogs 命名空间
﻿namespace MicaSetup.Shell.Dialogs;

# 内部静态类，包含 ExplorerBrowser 的 CLSID GUID
internal static class ExplorerBrowserCLSIDGuid
{
    # ExplorerBrowser 的 CLSID GUID 常量
    internal const string ExplorerBrowser = "71F96385-DDD6-48D3-A0C1-AE06E8B055FB";
}

# 内部静态类，包含各种 ExplorerBrowser 接口的 IID GUID
internal static class ExplorerBrowserIIDGuid
{
    # DShellFolderViewEvents 接口的 IID GUID 常量
    internal const string DShellFolderViewEvents = "62112AA2-EBE4-11cf-A5FB-0020AFE7292D";
    # ICommDlgBrowser 接口的 IID GUID 常量
    internal const string ICommDlgBrowser = "000214F1-0000-0000-C000-000000000046";
    # ICommDlgBrowser2 接口的 IID GUID 常量
    internal const string ICommDlgBrowser2 = "10339516-2894-11d2-9039-00C04F8EEB3E";
    # ICommDlgBrowser3 接口的 IID GUID 常量
    internal const string ICommDlgBrowser3 = "c8ad25a1-3294-41ee-8165-71174bd01c57";
    # IDispatch 接口的 IID GUID 常量
    internal const string IDispatch = "00020400-0000-0000-C000-000000000046";
    # IExplorerBrowser 接口的 IID GUID 常量
    internal const string IExplorerBrowser = "DFD3B6B5-C10C-4BE9-85F6-A66969F402F6";
    # IExplorerBrowserEvents 接口的 IID GUID 常量
    internal const string IExplorerBrowserEvents = "361bbdc7-e6ee-4e13-be58-58e2240c810f";
    # IExplorerPaneVisibility 接口的 IID GUID 常量
    internal const string IExplorerPaneVisibility = "e07010ec-bc17-44c0-97b0-46c7c95b9edc";
    # IFolderView 接口的 IID GUID 常量
    internal const string IFolderView = "cde725b0-ccc9-4519-917e-325d72fab4ce";
    # IFolderView2 接口的 IID GUID 常量
    internal const string IFolderView2 = "1af3a467-214f-4298-908e-06b03e0b39f9";
    # IInputObject 接口的 IID GUID 常量
    internal const string IInputObject = "68284fAA-6A48-11D0-8c78-00C04fd918b4";
    # IKnownFolderManager 接口的 IID GUID 常量
    internal const string IKnownFolderManager = "8BE2D872-86AA-4d47-B776-32CCA40C7018";
    # IServiceProvider 接口的 IID GUID 常量
    internal const string IServiceProvider = "6d5140c1-7436-11ce-8034-00aa006009fa";
    # IShellView 接口的 IID GUID 常量
    internal const string IShellView = "000214E3-0000-0000-C000-000000000046";
}

# 内部静态类，定义 ExplorerBrowser 视图的分发 ID
internal static class ExplorerBrowserViewDispatchIds
{
    # 内容更改的分发 ID
    internal const int ContentsChanged = 207;
    # 文件列表枚举完成的分发 ID
    internal const int FileListEnumDone = 201;
    # 选定项更改的分发 ID
    internal const int SelectedItemChanged = 220;
    # 选择更改的分发 ID
    internal const int SelectionChanged = 200;
}

# 内部静态类，定义 ExplorerBrowser 视图的面板 GUID
internal static class ExplorerBrowserViewPanes
{
    # 高级查询面板的 GUID
    internal const string AdvancedQuery = "b4e9db8b-34ba-4c39-b5cc-16a1bd2c411c";
    # 命令面板的 GUID
    internal const string Commands = "d9745868-ca5f-4a76-91cd-f5a129fbb076";
    # 组织命令面板的 GUID
    internal const string CommandsOrganize = "72e81700-e3ec-4660-bf24-3c3b7b648806";
    # 查看命令面板的 GUID
    internal const string CommandsView = "21f7c32d-eeaa-439b-bb51-37b96fd6a943";
    # 详细信息面板的 GUID
    internal const string Details = "43abf98b-89b8-472d-b9ce-e69b8229f019";
    # 导航面板的 GUID
    internal const string Navigation = "cb316b22-25f7-42b8-8a09-540d23a43c2f";
    # 预览面板的 GUID
    internal const string Preview = "893c63d1-45c8-4d17-be19-223be71be365";
    # 查询面板的 GUID
    internal const string Query = "65bcde4f-4f07-4f27-83a7-1afca4df7ddd";
}
```