# `.\better-genshin-impact\Build\MicaSetup\Natives\Shell\Dialogs\Common\IDialogControlHost.cs`

```cs
# 定义一个名为 MicaSetup.Shell.Dialogs 的命名空间
namespace MicaSetup.Shell.Dialogs;

# 定义一个接口 IDialogControlHost
public interface IDialogControlHost
{
    # 定义一个方法 ApplyCollectionChanged，用于应用集合更改
    void ApplyCollectionChanged();

    # 定义一个方法 ApplyControlPropertyChange，用于应用控件属性更改
    void ApplyControlPropertyChange(string propertyName, DialogControl control);

    # 定义一个方法 IsCollectionChangeAllowed，用于检查是否允许集合更改
    bool IsCollectionChangeAllowed();

    # 定义一个方法 IsControlPropertyChangeAllowed，用于检查是否允许控件属性更改
    bool IsControlPropertyChangeAllowed(string propertyName, DialogControl control);
}
```