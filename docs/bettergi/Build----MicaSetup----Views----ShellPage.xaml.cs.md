# `.\better-genshin-impact\Build\MicaSetup\Views\ShellPage.xaml.cs`

```cs
# 引入 MicaSetup.ViewModels 命名空间
﻿using MicaSetup.ViewModels;
# 引入 System.Windows.Controls 命名空间
using System.Windows.Controls;

# 定义 MicaSetup.Views 命名空间
namespace MicaSetup.Views;

# 定义 ShellPage 类，继承自 UserControl
public partial class ShellPage : UserControl
{
    # 定义一个只读属性 ViewModel，类型为 ShellViewModel
    public ShellViewModel ViewModel { get; }

    # ShellPage 类的构造函数
    public ShellPage()
    {
        # 初始化 DataContext 为新的 ShellViewModel 实例，并赋值给 ViewModel 属性
        DataContext = ViewModel = new();
        # 调用初始化组件方法，生成控件
        InitializeComponent();
    }
}
```