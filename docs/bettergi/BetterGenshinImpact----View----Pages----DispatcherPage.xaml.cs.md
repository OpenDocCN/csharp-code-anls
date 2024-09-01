# `.\better-genshin-impact\BetterGenshinImpact\View\Pages\DispatcherPage.xaml.cs`

```cs
# 引入 BetterGenshinImpact.ViewModel.Pages 命名空间
﻿using BetterGenshinImpact.ViewModel.Pages;

# 引入 BetterGenshinImpact.View.Pages 命名空间
namespace BetterGenshinImpact.View.Pages;

# 定义 DispatcherPage 类，部分定义
public partial class DispatcherPage
{
    # 定义 ViewModel 属性，用于绑定视图模型
    public DispatcherPageViewModel ViewModel { get; }

    # DispatcherPage 类的构造函数
    public DispatcherPage(DispatcherPageViewModel viewModel)
    {
        # 设置 DataContext 属性并初始化 ViewModel 属性
        DataContext = ViewModel = viewModel;
        # 调用 InitializeComponent 方法，初始化组件
        InitializeComponent();
    }
}
```