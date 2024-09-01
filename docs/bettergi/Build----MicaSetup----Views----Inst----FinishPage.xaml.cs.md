# `.\better-genshin-impact\Build\MicaSetup\Views\Inst\FinishPage.xaml.cs`

```cs
# 引入 MicaSetup.ViewModels 命名空间
﻿using MicaSetup.ViewModels;
# 引入 System.Windows.Controls 命名空间
using System.Windows.Controls;

# 定义 MicaSetup.Views 命名空间
namespace MicaSetup.Views;

# 定义一个名为 FinishPage 的类，继承自 UserControl
public partial class FinishPage : UserControl
{
    # 定义一个只读属性 ViewModel，类型为 FinishViewModel
    public FinishViewModel ViewModel { get; }

    # FinishPage 类的构造函数
    public FinishPage()
    {
        # 初始化 DataContext 属性为一个新的 FinishViewModel 实例，并赋值给 ViewModel 属性
        DataContext = ViewModel = new();
        # 初始化组件，通常用于加载 XAML 定义的 UI 元素
        InitializeComponent();
    }
}
```