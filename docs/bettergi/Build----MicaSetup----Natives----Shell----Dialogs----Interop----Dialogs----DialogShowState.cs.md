# `.\better-genshin-impact\Build\MicaSetup\Natives\Shell\Dialogs\Interop\Dialogs\DialogShowState.cs`

```cs
# 定义一个命名空间 MicaSetup.Shell.Dialogs，用于组织相关的类、接口和枚举
namespace MicaSetup.Shell.Dialogs;

# 定义一个公开的枚举类型 DialogShowState，用于表示对话框的显示状态
public enum DialogShowState
{
    # 对话框在显示之前的状态
    PreShow,
    # 对话框正在显示的状态
    Showing,
    # 对话框在关闭过程中的状态
    Closing,
    # 对话框已经关闭的状态
    Closed,
}
```