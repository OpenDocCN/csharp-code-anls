# `.\better-genshin-impact\Build\MicaSetup\Views\Inst\InstallPage.xaml.cs`

```cs
# 引入 MicaSetup.ViewModels 命名空间
﻿using MicaSetup.ViewModels;
# 引入 System.Windows.Controls 命名空间
using System.Windows.Controls;

# 定义 MicaSetup.Views 命名空间
namespace MicaSetup.Views;

# 定义一个 InstallPage 类，继承自 UserControl
public partial class InstallPage : UserControl
{
    # 声明一个只读属性 ViewModel 类型为 InstallViewModel
    public InstallViewModel ViewModel { get; }

    # InstallPage 构造函数
    public InstallPage()
    {
        # 初始化 ViewModel 属性并设置 DataContext 为新的 InstallViewModel 实例
        DataContext = ViewModel = new();
        # 初始化组件
        InitializeComponent();
    }
}
```