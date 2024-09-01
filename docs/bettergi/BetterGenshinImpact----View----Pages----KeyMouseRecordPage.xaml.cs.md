# `.\better-genshin-impact\BetterGenshinImpact\View\Pages\KeyMouseRecordPage.xaml.cs`

```cs
# 引入 BetterGenshinImpact.ViewModel.Pages 命名空间中的相关类
﻿using BetterGenshinImpact.ViewModel.Pages;
# 引入 System.Windows.Controls 命名空间中的控件类
using System.Windows.Controls;

# 定义 BetterGenshinImpact.View.Pages 命名空间
namespace BetterGenshinImpact.View.Pages
{
    # <summary> 标签用于为 KeyMouseRecordPage.xaml 的交互逻辑提供描述
    /// KeyMouseRecordPage.xaml 的交互逻辑
    /// </summary>
    # 定义一个部分类 KeyMouseRecordPage，继承自 Page 类
    public partial class KeyMouseRecordPage : Page
    {
        # 定义一个只读属性 ViewModel，用于存储 KeyMouseRecordPageViewModel 实例
        private KeyMouseRecordPageViewModel ViewModel { get; }

        # 构造函数，接受一个 KeyMouseRecordPageViewModel 类型的参数
        public KeyMouseRecordPage(KeyMouseRecordPageViewModel viewModel)
        {
            # 设置页面的数据上下文为传入的 viewModel，并初始化 ViewModel 属性
            DataContext = ViewModel = viewModel;
            # 调用初始化组件方法，设置页面的初始状态
            InitializeComponent();
        }
    }
}
```