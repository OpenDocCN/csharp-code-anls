# `.\better-genshin-impact\BetterGenshinImpact\Model\KeyMouseScriptItem.cs`

```cs
# 使用 CommunityToolkit.Mvvm.ComponentModel 中的功能
using CommunityToolkit.Mvvm.ComponentModel;
# 引入系统功能
using System;

# 定义 BetterGenshinImpact.Model 命名空间
namespace BetterGenshinImpact.Model;

# 定义一个可观察对象的部分类
public partial class KeyMouseScriptItem : ObservableObject
{
    # 定义一个可观察属性 _name，初始值为空字符串
    [ObservableProperty]
    private string _name = string.Empty;

    # 定义一个可观察属性 _createTimeStr，初始值为空字符串
    [ObservableProperty]
    private string _createTimeStr = string.Empty;

    # 定义一个公共属性 CreateTime，类型为 DateTime
    public DateTime CreateTime { get; set; }
}
```