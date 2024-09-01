# `.\better-genshin-impact\BetterGenshinImpact\View\Pages\ScriptControlPage.xaml.cs`

```cs
# 定义一个名为 ScriptControlPage 的部分类
public partial class ScriptControlPage
{
    # 定义一个只读属性 ViewModel，类型为 ScriptControlViewModel
    public ScriptControlViewModel ViewModel { get; }

    # 构造函数，接收一个 ScriptControlViewModel 类型的参数并初始化
    public ScriptControlPage(ScriptControlViewModel viewModel)
    {
        # 设置 DataContext 为传入的 viewModel，并将 ViewModel 属性初始化
        DataContext = ViewModel = viewModel;
        # 初始化组件，通常是加载 XAML 文件中的 UI 组件
        InitializeComponent();
    }
}
```