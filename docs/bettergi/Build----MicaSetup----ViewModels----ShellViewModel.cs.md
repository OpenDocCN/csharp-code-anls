# `.\better-genshin-impact\Build\MicaSetup\ViewModels\ShellViewModel.cs`

```cs
# 引入 CommunityToolkit.Mvvm.ComponentModel 命名空间中的功能
﻿using CommunityToolkit.Mvvm.ComponentModel;
# 引入 MicaSetup.Controls 命名空间中的控件
using MicaSetup.Controls;
# 引入 MicaSetup.Design.Controls 命名空间中的设计控件
using MicaSetup.Design.Controls;
# 引入 System.Linq 命名空间中的 LINQ 扩展方法
using System.Linq;

# 定义 MicaSetup.ViewModels 命名空间
namespace MicaSetup.ViewModels;

# 定义一个 ShellViewModel 类，继承自 ObservableObject 类
public partial class ShellViewModel : ObservableObject
{
    # 定义一个可观察属性 route，并初始化为 null
    [ObservableProperty]
    private string route = null!;

    # 构造函数
    public ShellViewModel()
    {
        # 注册路由
        Routing.RegisterRoute();
        # 设置 Route 属性为 ShellPageSetting.PageDict 中的第一个键
        Route = ShellPageSetting.PageDict.FirstOrDefault().Key;
    }
}
```