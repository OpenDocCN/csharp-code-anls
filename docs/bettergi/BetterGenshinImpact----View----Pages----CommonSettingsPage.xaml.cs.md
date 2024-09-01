# `.\better-genshin-impact\BetterGenshinImpact\View\Pages\CommonSettingsPage.xaml.cs`

```cs
# 引入 BetterGenshinImpact.ViewModel.Pages 命名空间中的类
﻿using BetterGenshinImpact.ViewModel.Pages;
# 引入 System.Windows.Controls 命名空间中的类
using System.Windows.Controls;

# 定义 BetterGenshinImpact.View.Pages 命名空间
namespace BetterGenshinImpact.View.Pages
{
    # <summary> 标签内描述该类为 CommonSettingsPage.xaml 的交互逻辑
    /// <summary>
    /// CommonSettingsPage.xaml 的交互逻辑
    /// </summary>
    # 定义一个公共类 CommonSettingsPage，继承自 Page 类
    public partial class CommonSettingsPage : Page
    {
        # 定义一个只读属性 ViewModel，类型为 CommonSettingsPageViewModel
        CommonSettingsPageViewModel ViewModel { get; }
        # 构造函数，接受一个 CommonSettingsPageViewModel 类型的参数
        public CommonSettingsPage(CommonSettingsPageViewModel viewModel)
        {
            # 设置页面的数据上下文为传入的 ViewModel
            DataContext = ViewModel = viewModel;
            # 初始化组件，通常是用于设置 XAML 中定义的 UI 元素
            InitializeComponent();
        }
    }
}
```