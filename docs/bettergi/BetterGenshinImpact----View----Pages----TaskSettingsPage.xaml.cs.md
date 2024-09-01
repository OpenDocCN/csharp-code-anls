# `.\better-genshin-impact\BetterGenshinImpact\View\Pages\TaskSettingsPage.xaml.cs`

```cs
using BetterGenshinImpact.ViewModel.Pages; // 引用 BetterGenshinImpact.ViewModel.Pages 命名空间
using System.Windows.Controls; // 引用 System.Windows.Controls 命名空间

namespace BetterGenshinImpact.View.Pages; // 定义 BetterGenshinImpact.View.Pages 命名空间

/// <summary>
/// TaskSettingsPage.xaml 的交互逻辑
/// </summary>
public partial class TaskSettingsPage : Page // 定义一个 TaskSettingsPage 类，继承自 Page
{

    TaskSettingsPageViewModel ViewModel { get; } // 声明只读属性 ViewModel，类型为 TaskSettingsPageViewModel

    public TaskSettingsPage(TaskSettingsPageViewModel viewModel) // 构造函数，接受一个 TaskSettingsPageViewModel 对象作为参数
    {
        DataContext = ViewModel = viewModel; // 设置数据上下文为传入的视图模型，方便进行数据绑定
        InitializeComponent(); // 初始化组件，生成 XAML 中定义的界面元素
    }
}
```