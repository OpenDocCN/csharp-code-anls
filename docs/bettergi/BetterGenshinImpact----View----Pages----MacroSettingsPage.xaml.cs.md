# `.\better-genshin-impact\BetterGenshinImpact\View\Pages\MacroSettingsPage.xaml.cs`

```cs
# 使用 BetterGenshinImpact.ViewModel.Pages 命名空间中的类
﻿using BetterGenshinImpact.ViewModel.Pages;
# 引入 WPF 的控件类
using System.Windows.Controls;

# 定义 BetterGenshinImpact.View.Pages 命名空间
namespace BetterGenshinImpact.View.Pages
{
    /// <summary>
    /// MacroSettingsPage.xaml 的交互逻辑
    /// </summary>
    # 定义一个名为 MacroSettingsPage 的部分类，继承自 Page 类
    public partial class MacroSettingsPage : Page
    {
        # 定义一个只读属性 ViewModel，类型为 MacroSettingsPageViewModel
        MacroSettingsPageViewModel ViewModel { get; }

        # 定义构造函数，接受一个 MacroSettingsPageViewModel 类型的参数
        public MacroSettingsPage(MacroSettingsPageViewModel viewModel)
        {
            # 将传入的 ViewModel 参数赋值给 ViewModel 属性，并设置 DataContext
            DataContext = ViewModel = viewModel;
            # 初始化组件，加载 XAML 文件中的控件
            InitializeComponent();
        }
    }
}
```