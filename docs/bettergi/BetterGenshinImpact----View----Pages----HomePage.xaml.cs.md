# `.\better-genshin-impact\BetterGenshinImpact\View\Pages\HomePage.xaml.cs`

```cs
# 引入 BetterGenshinImpact.ViewModel.Pages 命名空间
﻿using BetterGenshinImpact.ViewModel.Pages;

# 定义 BetterGenshinImpact.View.Pages 命名空间
namespace BetterGenshinImpact.View.Pages
{
    # 定义 HomePage 类
    public partial class HomePage
    {
        # 定义只读属性 ViewModel，用于绑定视图模型
        public HomePageViewModel ViewModel { get; }

        # HomePage 构造函数
        public HomePage(HomePageViewModel viewModel, HotKeyPageViewModel hotKeyPageViewModel)
        {
            # 设置 DataContext 为传入的 viewModel，并将 ViewModel 属性初始化为该 viewModel
            DataContext = ViewModel = viewModel;
            # 初始化组件
            InitializeComponent();

            # 注释说明 hotKeyPageViewModel 用于初始化热键
        }
    }
}
```