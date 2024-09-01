# `.\better-genshin-impact\Build\MicaSetup\Views\Uninst\FinishPage.xaml.cs`

```cs
# 引入 MicaSetup.ViewModels 命名空间，允许使用其中定义的类
﻿using MicaSetup.ViewModels;
# 引入 System.Windows.Controls 命名空间，允许使用控件类
using System.Windows.Controls;

# 定义 MicaSetup.Views 命名空间
namespace MicaSetup.Views;

# 定义一个 FinishPage 类，该类继承自 UserControl 类
public partial class FinishPage : UserControl
{
    # 定义一个 FinishViewModel 类型的属性 ViewModel
    public FinishViewModel ViewModel { get; }

    # FinishPage 类的构造函数
    public FinishPage()
    {
        # 设置 DataContext 属性为新创建的 FinishViewModel 实例，并赋值给 ViewModel 属性
        DataContext = ViewModel = new();
        # 初始化组件
        InitializeComponent();
    }
}
```