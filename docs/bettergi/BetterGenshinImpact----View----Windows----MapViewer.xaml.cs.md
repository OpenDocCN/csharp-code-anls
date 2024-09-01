# `.\better-genshin-impact\BetterGenshinImpact\View\Windows\MapViewer.xaml.cs`

```cs
# 引入 BetterGenshinImpact.ViewModel.Windows 命名空间
﻿using BetterGenshinImpact.ViewModel.Windows;

# 引入 BetterGenshinImpact.View.Windows 命名空间
namespace BetterGenshinImpact.View.Windows;

# 定义 MapViewer 类
public partial class MapViewer
{
    # 定义 ViewModel 属性
    public MapViewerViewModel ViewModel { get; }

    # MapViewer 构造函数
    public MapViewer()
    {
        # 设置 DataContext 属性并初始化 ViewModel 实例
        DataContext = ViewModel = new();
        # 初始化组件
        InitializeComponent();
    }
}
```