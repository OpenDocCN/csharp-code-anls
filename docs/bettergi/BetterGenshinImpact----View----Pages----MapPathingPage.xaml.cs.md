# `.\better-genshin-impact\BetterGenshinImpact\View\Pages\MapPathingPage.xaml.cs`

```cs
# 导入 BetterGenshinImpact.ViewModel 命名空间
﻿using BetterGenshinImpact.ViewModel;
# 导入 BetterGenshinImpact.ViewModel.Pages 命名空间
using BetterGenshinImpact.ViewModel.Pages;
# 导入 System.Windows.Controls 命名空间，用于处理控件
using System.Windows.Controls;

# 定义 BetterGenshinImpact.View.Pages 命名空间
namespace BetterGenshinImpact.View.Pages
{
    /// <summary>
    /// JsListPage.xaml 的交互逻辑
    /// </summary>
    # 定义 MapPathingPage 类，继承自 Page 类
    public partial class MapPathingPage : Page
    {
        # 定义只读属性 ViewModel，用于绑定数据
        private MapPathingViewModel ViewModel { get; }

        # 构造函数，接收一个 MapPathingViewModel 对象作为参数
        public MapPathingPage(MapPathingViewModel viewModel)
        {
            # 设置 DataContext 为传入的 viewModel，并初始化 ViewModel 属性
            DataContext = ViewModel = viewModel;
            # 初始化组件，加载 XAML 资源
            InitializeComponent();
        }
    }
}
```