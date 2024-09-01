# `.\better-genshin-impact\BetterGenshinImpact\View\Pages\HotkeyPage.xaml.cs`

```cs
# 引入 BetterGenshinImpact.ViewModel.Pages 命名空间
﻿using BetterGenshinImpact.ViewModel.Pages;
# 引入 System.Windows.Controls 命名空间
using System.Windows.Controls;

# 定义 BetterGenshinImpact.View.Pages 命名空间
namespace BetterGenshinImpact.View.Pages
{
    /// <summary>
    /// TaskSettingsPage.xaml 的交互逻辑
    /// </summary>
    # 定义 HotKeyPage 类，继承自 Page 类
    public partial class HotKeyPage : Page
    {
        # 定义只读属性 ViewModel，类型为 HotKeyPageViewModel
        private HotKeyPageViewModel ViewModel { get; }

        # 构造函数，接受一个 HotKeyPageViewModel 类型的参数 viewModel
        public HotKeyPage(HotKeyPageViewModel viewModel)
        {
            # 设置 DataContext 属性为传入的 viewModel，并赋值给 ViewModel 属性
            DataContext = ViewModel = viewModel;
            # 初始化组件
            InitializeComponent();
        }
    }
}
```