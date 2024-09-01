# `.\better-genshin-impact\Build\MicaSetup\Views\Uninst\UninstallPage.xaml.cs`

```cs
# 引入 MicaSetup.ViewModels 命名空间
﻿using MicaSetup.ViewModels;
# 引入 System.Windows.Controls 命名空间
using System.Windows.Controls;

# 定义 MicaSetup.Views 命名空间
namespace MicaSetup.Views;

# 定义 UninstallPage 类，继承自 UserControl
public partial class UninstallPage : UserControl
{
    # 声明 ViewModel 属性，只读
    public UninstallViewModel ViewModel { get; }

    # UninstallPage 类的构造函数
    public UninstallPage()
    {
        # 设置数据上下文，并初始化 ViewModel 实例
        DataContext = ViewModel = new();
        # 初始化组件
        InitializeComponent();
    }
}
```