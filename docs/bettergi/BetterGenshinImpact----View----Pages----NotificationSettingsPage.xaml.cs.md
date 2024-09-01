# `.\better-genshin-impact\BetterGenshinImpact\View\Pages\NotificationSettingsPage.xaml.cs`

```cs
# 导入 BetterGenshinImpact.ViewModel.Pages 命名空间中的类
﻿using BetterGenshinImpact.ViewModel.Pages;
# 导入 System.Windows.Controls 命名空间中的控件
using System.Windows.Controls;

# 定义 BetterGenshinImpact.View.Pages 命名空间
namespace BetterGenshinImpact.View.Pages
{
    # <summary>
    # NotificationSettingsPage.xaml 的交互逻辑
    # </summary>
    # 定义一个部分类 NotificationSettingsPage，继承自 Page 类
    public partial class NotificationSettingsPage : Page
    {
        # 定义只读属性 ViewModel，用于存储页面的视图模型
        private NotificationSettingsPageViewModel ViewModel { get; }

        # 构造函数，接收一个 NotificationSettingsPageViewModel 实例并初始化视图模型
        public NotificationSettingsPage(NotificationSettingsPageViewModel viewModel)
        {
            # 设置 DataContext 为传入的视图模型，并初始化 ViewModel 属性
            DataContext = ViewModel = viewModel;
            # 调用初始化组件的方法，加载 XAML 文件中的定义
            InitializeComponent();
        }
    }
}
```