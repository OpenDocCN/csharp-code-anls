# `.\better-genshin-impact\Build\MicaSetup\AssemblyInfo.cs`

```cs
# 引入用于反射操作的命名空间
using System.Reflection;
# 引入用于与 COM 交互的命名空间
using System.Runtime.InteropServices;
# 引入用于 WPF 窗体应用程序的命名空间
using System.Windows;
# 引入用于 XAML 标记扩展的命名空间
using System.Windows.Markup;
# 引入用于 WPF 图形的命名空间
using System.Windows.Media;

# 禁用 DPI 感知
[assembly: DisableDpiAwareness]
# 指定 WPF 主题资源字典的位置
[assembly: ThemeInfo(ResourceDictionaryLocation.None, ResourceDictionaryLocation.SourceAssembly)]
# 指示程序集将被混淆
[assembly: ObfuscateAssembly(true)]
# 设置程序集的 COM 可见性为 false
[assembly: ComVisible(false)]
# 指定程序集的配置字符串
[assembly: AssemblyConfiguration("")]
# 指定程序集的商标字符串
[assembly: AssemblyTrademark("")]
# 指定程序集的文化字符串
[assembly: AssemblyCulture("")]
# 指定 XAML 命名空间前缀
[assembly: XmlnsPrefix("urn:MicaSetup", "mica")]
# 指定 XAML 命名空间定义的程序集
[assembly: XmlnsDefinition("urn:MicaSetup", "MicaSetup.Design.Controls")]
# 指定 XAML 命名空间定义的程序集
[assembly: XmlnsDefinition("urn:MicaSetup", "MicaSetup.Design.Converters")]
```