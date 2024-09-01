# `.\better-genshin-impact\Build\MicaSetup\Design\ResourceResourceDictionary.cs`

```cs
# 引入系统命名空间
using System;
# 引入 WPF 命名空间，用于处理窗口应用程序的资源
using System.Windows;

# 定义 MicaSetup.Design 命名空间
namespace MicaSetup.Design;

# 定义一个密封类，继承自 ResourceDictionary
public sealed class ResourceResourceDictionary : ResourceDictionary
{
    # 构造函数
    public ResourceResourceDictionary()
    {
        # 设置 ResourceDictionary 的 Source 属性为指定 URI，指向资源文件
        Source = new Uri($"pack://application:,,,/MicaSetup;component/Resources/Resource.xaml");
    }
}
```