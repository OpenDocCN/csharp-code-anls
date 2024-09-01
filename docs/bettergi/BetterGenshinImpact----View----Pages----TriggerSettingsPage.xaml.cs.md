# `.\better-genshin-impact\BetterGenshinImpact\View\Pages\TriggerSettingsPage.xaml.cs`

```cs
# 定义命名空间 BetterGenshinImpact.View.Pages
namespace BetterGenshinImpact.View.Pages;

# 定义一个名为 TriggerSettingsPage 的部分类
public partial class TriggerSettingsPage
{
    # 定义一个只读属性 ViewModel 类型为 TriggerSettingsPageViewModel
    private TriggerSettingsPageViewModel ViewModel { get; }

    # 构造函数，接收一个 TriggerSettingsPageViewModel 类型的参数 viewModel
    public TriggerSettingsPage(TriggerSettingsPageViewModel viewModel)
    {
        # 将参数 viewModel 赋值给 ViewModel 属性
        DataContext = ViewModel = viewModel;
        # 初始化组件
        InitializeComponent();
    }
}
```