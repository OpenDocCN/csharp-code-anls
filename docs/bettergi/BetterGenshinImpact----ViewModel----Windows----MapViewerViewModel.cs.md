# `.\better-genshin-impact\BetterGenshinImpact\ViewModel\Windows\MapViewerViewModel.cs`

```cs
# 导入 BetterGenshinImpact.Core.Config 命名空间
﻿using BetterGenshinImpact.Core.Config;

# 导入 CommunityToolkit.Mvvm.ComponentModel 命名空间
using CommunityToolkit.Mvvm.ComponentModel;

# 导入 CommunityToolkit.Mvvm.Messaging 命名空间
using CommunityToolkit.Mvvm.Messaging;

# 导入 CommunityToolkit.Mvvm.Messaging.Messages 命名空间
using CommunityToolkit.Mvvm.Messaging.Messages;

# 导入 System.Windows 命名空间
using System.Windows;

# 定义命名空间 BetterGenshinImpact.ViewModel.Windows
namespace BetterGenshinImpact.ViewModel.Windows;

# 定义 MapViewerViewModel 类，继承自 ObservableObject
public partial class MapViewerViewModel : ObservableObject
{
    # 定义可观察属性 _bigMapRect，初始化为 (0, 0, 0, 0) 的矩形
    [ObservableProperty]
    private Rect _bigMapRect = new(0, 0, 0, 0);

    # 定义可观察属性 _mapPath，初始化为全局绝对路径的地图图片
    [ObservableProperty]
    private string _mapPath = Global.Absolute(@"Assets\Map\mainMap100Block.png");

    # MapViewerViewModel 构造函数
    public MapViewerViewModel()
    {
        # 注册对 PropertyChangedMessage<object> 消息的处理
        WeakReferenceMessenger.Default.Register<PropertyChangedMessage<object>>(this, (sender, msg) =>
        {
            # 如果消息属性名为 "UpdateBigMapRect"，则更新 BigMapRect 属性
            if (msg.PropertyName == "UpdateBigMapRect")
            {
                BigMapRect = (Rect)msg.NewValue;
            }
        });
    }
}
```