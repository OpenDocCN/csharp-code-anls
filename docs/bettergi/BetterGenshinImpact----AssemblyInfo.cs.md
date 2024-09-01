# `.\better-genshin-impact\BetterGenshinImpact\AssemblyInfo.cs`

```cs
# 指定使用 Windows Presentation Foundation (WPF) 的程序集
using System.Windows;
using System.Windows.Media;

# 禁用 DPI 感知，以防系统缩放影响应用程序的外观
[assembly: DisableDpiAwareness]
# 指定主题资源字典的位置，None 表示没有主题资源字典，SourceAssembly 表示资源字典位于当前程序集
[assembly: ThemeInfo(ResourceDictionaryLocation.None, ResourceDictionaryLocation.SourceAssembly)]
```