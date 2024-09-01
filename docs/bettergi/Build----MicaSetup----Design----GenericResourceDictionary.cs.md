# `.\better-genshin-impact\Build\MicaSetup\Design\GenericResourceDictionary.cs`

```cs
# 引入必要的命名空间
﻿using System;
using System.Windows;

# 定义命名空间
namespace MicaSetup.Design;

# 定义一个继承自 ResourceDictionary 的密封类
public sealed class GenericResourceDictionary : ResourceDictionary
{
    # 构造函数
    public GenericResourceDictionary()
    {
        # 设置资源字典的来源 URI，指向 Generic.xaml 文件
        Source = new Uri($"pack://application:,,,/MicaSetup;component/Resources/Generic.xaml");
    }
}
```