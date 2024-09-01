# `.\better-genshin-impact\Build\MicaSetup\App.xaml.cs`

```cs
# 引入 MicaSetup.Helper 命名空间
﻿using MicaSetup.Helper;
# 引入 System.Windows 命名空间
using System.Windows;

# 定义 MicaSetup 命名空间
namespace MicaSetup;

# 定义 App 类，继承自 Application 和 IApp 接口
public partial class App : Application, IApp
{
    # App 类的构造函数
    public App()
    {
        # 初始化组件
        InitializeComponent();
    }
}

# 定义 IApp 接口
public interface IApp
{
    # 定义 Run 方法，返回整数
    public int Run();
}
```