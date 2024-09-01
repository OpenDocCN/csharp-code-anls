# `.\better-genshin-impact\Build\MicaSetup\ViewModels\Inst\InstallViewModel.cs`

```cs
# 引入 CommunityToolkit.Mvvm.ComponentModel 命名空间，提供数据绑定和通知功能
﻿using CommunityToolkit.Mvvm.ComponentModel;
# 引入 MicaSetup.Design.Controls 命名空间，可能包含自定义控件
using MicaSetup.Design.Controls;
# 引入 MicaSetup.Helper 命名空间，可能包含帮助工具类
using MicaSetup.Helper;
# 引入 MicaSetup.Helper.Helper 命名空间，可能包含额外的帮助工具类
using MicaSetup.Helper.Helper;
# 引入 MicaSetup.Services 命名空间，可能包含服务相关的类
using MicaSetup.Services;
# 引入 System 命名空间，提供基本的系统功能
using System;
# 引入 System.IO 命名空间，提供文件和流操作功能
using System.IO;
# 引入 System.Threading.Tasks 命名空间，提供异步编程功能
using System.Threading.Tasks;

# 定义一个命名空间 MicaSetup.ViewModels，用于组织视图模型相关的类
namespace MicaSetup.ViewModels;

# 禁用 CS0162 编译警告，表示代码存在不可达代码的警告
#pragma warning disable CS0162
# 禁用 IDE0035 编译警告，表示建议简化代码的警告
#pragma warning disable IDE0035

# 定义一个 InstallViewModel 类，继承自 ObservableObject，支持属性通知
public partial class InstallViewModel : ObservableObject
{
    # 定义一个 installInfo 字段，绑定到 UI 的数据属性，初始值为空字符串
    [ObservableProperty]
    private string installInfo = string.Empty;

    # 定义一个 installProgress 字段，绑定到 UI 的数据属性，初始值为 0d
    [ObservableProperty]
    private double installProgress = 0d;

    # 定义 InstallViewModel 类的构造函数
    public InstallViewModel()
    }
}
```