# `.\better-genshin-impact\Build\MicaSetup\Views\Uninst\MainPage.xaml.cs`

```cs
# 引入 MicaSetup.ViewModels 命名空间，供后续使用
﻿using MicaSetup.ViewModels;
# 引入 System.Windows.Controls 命名空间，用于创建用户控件
using System.Windows.Controls;

# 定义 MicaSetup.Views 命名空间
namespace MicaSetup.Views;

# 定义 MainPage 类，继承自 UserControl 类
public partial class MainPage : UserControl
{
    # 定义只读属性 ViewModel，类型为 MainViewModel
    public MainViewModel ViewModel { get; }

    # MainPage 类的构造函数
    public MainPage()
    {
        # 将 ViewModel 属性初始化为新的 MainViewModel 实例，并将 DataContext 设置为该实例
        DataContext = ViewModel = new();
        # 调用 InitializeComponent 方法，初始化组件
        InitializeComponent();
    }
}
```