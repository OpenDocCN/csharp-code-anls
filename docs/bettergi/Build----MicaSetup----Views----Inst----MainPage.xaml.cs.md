# `.\better-genshin-impact\Build\MicaSetup\Views\Inst\MainPage.xaml.cs`

```cs
# 使用 MicaSetup.ViewModels 命名空间
using MicaSetup.ViewModels;
# 使用 System.Windows.Controls 命名空间
using System.Windows.Controls;

# 定义 MainPage 类，继承自 UserControl
namespace MicaSetup.Views;

public partial class MainPage : UserControl
{
    # 定义只读属性 ViewModel，类型为 MainViewModel
    public MainViewModel ViewModel { get; }

    # MainPage 类的构造函数
    public MainPage()
    {
        # 设置 DataContext 为 ViewModel 实例
        DataContext = ViewModel = new();
        # 初始化组件
        InitializeComponent();
    }
}
```