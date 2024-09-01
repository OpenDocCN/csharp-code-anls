# `.\better-genshin-impact\BetterGenshinImpact\View\Pages\JsListPage.xaml.cs`

```cs
﻿using BetterGenshinImpact.ViewModel; // 引用 BetterGenshinImpact.ViewModel 命名空间
using BetterGenshinImpact.ViewModel.Pages; // 引用 BetterGenshinImpact.ViewModel.Pages 命名空间
using System.Windows.Controls; // 引用 System.Windows.Controls 命名空间，用于 WPF 控件

namespace BetterGenshinImpact.View.Pages; // 定义 BetterGenshinImpact.View.Pages 命名空间

/// <summary>
/// JsListPage.xaml 的交互逻辑
/// </summary>
public partial class JsListPage : Page // 定义 JsListPage 类，继承自 Page 类
{
    private JsListViewModel ViewModel { get; } // 定义只读属性 ViewModel 类型为 JsListViewModel

    public JsListPage(JsListViewModel viewModel) // JsListPage 类的构造函数，接受一个 JsListViewModel 类型的参数
    {
        DataContext = ViewModel = viewModel; // 将构造函数参数 viewModel 赋值给 ViewModel 属性，并设置 DataContext
        InitializeComponent(); // 调用 InitializeComponent 方法初始化组件
    }
}
```